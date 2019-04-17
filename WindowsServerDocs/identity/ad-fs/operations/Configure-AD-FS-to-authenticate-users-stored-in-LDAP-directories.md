---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: "Configurer ADFS pour authentifier les utilisateurs stockés dans les annuaires LDAP"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f8b8991e664a84c3f2b3200de4068af8d1476a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>Configurer ADFS pour authentifier les utilisateurs stockés dans les annuaires LDAP

>S’applique à: Windows Server2016

La rubrique suivante décrit la configuration requise pour activer votre infrastructure ADFS authentifier les utilisateurs dont les identités sont stockées dans les répertoires v3 conforme Lightweight Directory Access Protocol LDAP ().

Dans de nombreuses organisations, les solutions de gestion des identités se composent d’une combinaison d’ActiveDirectory, ADLDS ou les annuaires LDAP tiers. Avec la prise en charge ADFS pour authentifier les utilisateurs stockés dans des annuaires LDAP v3 conforme, vous pouvez bénéficier de l’ensemble de niveau entreprise ADFS de fonctionnalité ensemble, quelle que soit le stockage des identités d’utilisateur. ADFS prend en charge n’importe quel annuaire v3 compatible LDAP.

> [!NOTE]
> Certaines des fonctionnalités ADFS incluent l’authentification unique (SSO), l’authentification des appareils, les stratégies d’accès conditionnel flexible, prise en charge de travail-de-n’importe quel endroit par le biais de l’intégration avec le Proxy d’Application Web et la fédération transparente avec Azure AD qui à son tour permet de vous et vos utilisateurs d’utiliser le cloud, notamment Office 365 et autres applications SaaS.  Pour plus d’informations, voir [vue d’ensemble de ActiveDirectory Federation Services ](../../ad-fs/AD-FS-2016-Overview.md).

Dans l’ordre pour ADFS authentifier les utilisateurs à partir d’un annuaire LDAP, vous devez vous connecter cet annuaire LDAP pour votre batterie ADFS en créant un **approbation de fournisseur de revendications local **.  Une approbation de fournisseur de revendications local est un objet d’approbation qui représente un annuaire LDAP dans votre batterie ADFS. Un objet se compose d’un ensemble de règles qui identifient cet annuaire LDAP au service de fédération local, les noms et les identificateurs d’approbation de fournisseur de revendications.

Vous pouvez prendre en charge plusieurs annuaires LDAP, chacun avec son propre configuration, au sein de la même batterie de serveurs ADFS en ajoutant plusieurs **approbations de fournisseur de revendications local **. En outre, les forêts ADDS qui ne sont pas approuvées par ADFS réside dans la forêt peuvent être modélisées également en tant que les approbations de fournisseur de revendications local. Vous pouvez créer des approbations de fournisseur de revendications local à l’aide de Windows PowerShell.

Les annuaires LDAP (approbations de fournisseur de revendications local) peuvent coexister avec les annuaires ActiveDirectory (approbations de fournisseur de revendications) sur le même serveur ADFS, dans la même batterie de serveurs ADFS, par conséquent, une seule instance d’ADFS est capable d’authentifier et autoriser l’accès pour les utilisateurs qui sont stockés à la fois dans ActiveDirectory et les annuaires non-Active Directory.

Uniquement l’authentification par formulaire est pris en charge pour l’authentification des utilisateurs des annuaires LDAP. L’authentification Windows intégrée et basée sur les certificats ne sont pas pris en charge pour l’authentification des utilisateurs dans les annuaires LDAP.

Tous les protocoles passif d’autorisation qui sont pris en charge par ADFS, y compris SAML, WS-Federation, et OAuth sont également pris en charge pour les identités qui sont stockées dans des annuaires LDAP.

Le protocole d’autorisation active WS-Trust est également pris en charge pour les identités qui sont stockées dans des annuaires LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurer ADFS pour authentifier les utilisateurs stockés dans un annuaire LDAP
Pour configurer votre batterie ADFS pour authentifier les utilisateurs à partir d’un annuaire LDAP, vous pouvez effectuer les étapes suivantes:

1.  Tout d’abord, configurez une connexion à votre annuaire LDAP à l’aide du **New-AdfsLdapServerConnection** applet de commande:

    ```
    $DirectoryCred = Get-Credential
    $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
    ```

    > [!NOTE]
    > Il est recommandé de créer un nouvel objet de connexion pour chaque serveur LDAP que vous souhaitez vous connecter. ADFS peut se connecter à plusieurs serveurs LDAP de réplica et un basculement automatique en cas d’un serveur LDAP spécifique est arrêté. Pour ce cas, vous pouvez créer des un AdfsLdapServerConnection pour chacun de ces serveurs LDAP de réplica et puis ajouter le tableau d’objets de connexion à l’aide de-**LdapServerConnection** paramètre de la **Add-AdfsLocalClaimsProviderTrust** applet de commande.

    **Remarque:** votre tentative d’utilisation Get-Credential et tapez un nom unique et un mot de passe à utiliser pour établir une liaison à une instance LDAP peut entraîner un échec car le de la spécification d’interface utilisateur pour les formats d’entrée spécifiques, par exemple, DOMAINE\nom d’utilisateur ou user@domain.tld. Vous pouvez utiliser à la place de l’applet de commande ConvertTo-SecureString comme suit (l’exemple suivant suppose uid = admin, unité d’organisation = système en tant que le nom unique des informations d’identification à utiliser pour établir une liaison à l’instance LDAP):

    ```
    $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
    $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
    ```

    Puis entrez le mot de passe pour l’uid = admin et remplissez le reste des étapes.

2.  Ensuite, vous pouvez effectuer l’étape facultative de mappage d’attributs LDAP pour les revendications ADFS existantes à l’aide de la **New-AdfsLdapAttributeToClaimMapping** applet de commande. Dans l’exemple ci-dessous, vous mappez givenName, nom de famille, et les attributs de CommonName LDAP pour les revendications ADFS:

    ```
    #Map given name claim
    $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
    # Map surname claim
    $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
    # Map common name claim
    $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
    ```

    Ce mappage est effectué afin de rendre disponible en tant que revendications dans ADFS pour créer des règles de contrôle d’accès conditionnel dans ADFS des attributs à partir du magasin LDAP. Il permet également d’ADFS fonctionner avec des schémas personnalisés dans des magasins de LDAP en fournissant un moyen simple de mapper les attributs LDAP pour les revendications.

3.  Enfin, vous devez enregistrer le magasin de LDAP avec ADFS comme une approbation de fournisseur à l’aide de revendications le **Add-AdfsLocalClaimsProviderTrust** applet de commande:

    ```
    Add-AdfsLocalClaimsProviderTrust -Name "Vendors" -Identifier "urn:vendors" -Type Ldap

    # Connection info
    -LdapServerConnection $vendorDirectory 

    # How to locate user objects in directory
    -UserObjectClass inetOrgPerson -UserContainer "CN=VendorsContainer,CN=VendorsPartition" -LdapAuthenticationMethod Basic 

    # Claims for authenticated users
    -AnchorClaimLdapAttribute mail -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -LdapAttributeToClaimMapping @($GivenName, $Surname, $CommonName) 

    # General claims provider properties
    -AcceptanceTransformRules "c:[Type != ''] => issue(claim=c);" -Enabled $true 

    # Optional - supply user name suffix if you want to use Ws-Trust
    -OrganizationalAccountSuffix "vendors.contoso.com"

    ```

    Dans l’exemple ci-dessus, vous créez une approbation de fournisseur de revendications local appelée «Éditeurs». Vous spécifiez des informations de connexion ADFS pour se connecter à l’annuaire LDAP cette approbation de fournisseur de revendications local représente en affectant `$vendorDirectory`à la `-LdapServerConnection`paramètre. Notez que dans l’étape1, vous avez affecté `$vendorDirectory`une chaîne de connexion à utiliser lors de la connexion à votre annuaire LDAP spécifique. Enfin, vous devez spécifier le `$GivenName`, `$Surname`, et `$CommonName`les attributs LDAP (qui vous mappés sur les revendications ADFS) doivent être utilisés pour le contrôle d’accès conditionnel, y compris les stratégies d’authentification multifacteur et les règles d’autorisation d’émission, ainsi que pour d’émission via des revendications dans des jetons ADFS émis par la sécurité. Pour pouvoir utiliser les protocoles tels que Ws-Trust avec ADFS, vous devez spécifier le paramètre OrganizationalAccountSuffix, qui permet à ADFS éliminer l’ambiguïté entre les approbations de fournisseur de revendications local lors de la maintenance d’une demande d’autorisation active.

## <a name="see-also"></a>Voir aussi
[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md)


