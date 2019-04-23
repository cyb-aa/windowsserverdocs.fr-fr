---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: Configuration d'AD FS pour authentifier les utilisateurs stockées dans des annuaires LDAP
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f8b8991e664a84c3f2b3200de4068af8d1476a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846610"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>Configuration d'AD FS pour authentifier les utilisateurs stockées dans des annuaires LDAP

>S'applique à : Windows Server 2016

La rubrique suivante décrit la configuration requise pour activer votre infrastructure AD FS authentifier les utilisateurs dont les identités sont stockées dans les répertoires compatibles v3 Lightweight Directory Access Protocol (LDAP).

Dans de nombreuses organisations, les solutions de gestion des identités se composent d’une combinaison d’Active Directory, AD LDS ou des annuaires LDAP tiers. Avec l’ajout de prise en charge AD FS pour authentifier les utilisateurs stockés dans des annuaires LDAP v3 compatible, vous pouvez bénéficier de l’entreprise entière AD FS fonctionnalité définie, quel que soit le stockage des identités d’utilisateur. AD FS prend en charge n’importe quel répertoire compatibles v3 LDAP.

> [!NOTE]
> Certaines des fonctionnalités AD FS incluent session unique (SSO), l’authentification des appareils, les stratégies d’accès conditionnel flexible, prise en charge de travail-de-n’importe où grâce à l’intégration avec le Proxy d’Application Web et transparente la fédération avec Azure AD, qui à son tour permet de vous et vos utilisateurs d’utiliser le cloud, y compris Office 365 et autres applications SaaS.  Pour plus d’informations, consultez [présentation des Services de fédération Active Directory](../../ad-fs/AD-FS-2016-Overview.md).

Dans l’ordre pour AD FS authentifier les utilisateurs à partir d’un annuaire LDAP, vous devez vous connecter ce répertoire LDAP à votre batterie de serveurs AD FS en créant un **approbation de fournisseur de revendications local**.  Une approbation de fournisseur de revendications local est un objet d’approbation qui représente un répertoire LDAP dans votre batterie de serveurs AD FS. Un objet se compose d’un ensemble de règles qui identifient cet annuaire LDAP pour le service de fédération local, les noms et les identificateurs d’approbation de fournisseur de revendications.

Vous pouvez prendre en charge plusieurs annuaires LDAP, chacun avec sa propre configuration, au sein de la même batterie de serveurs AD FS en ajoutant plusieurs **approbations de fournisseur de revendications local**. En outre, les forêts AD DS qui ne sont pas approuvés par la forêt AD FS se trouve dans peuvent également être modélisés en tant que les approbations de fournisseur de revendications local. Vous pouvez créer des approbations de fournisseur de revendications local à l’aide de Windows PowerShell.

Les annuaires LDAP (approbations de fournisseur de revendications local) peuvent coexister avec les annuaires AD (approbations de fournisseur de revendications) sur le même serveur AD FS, dans la même batterie AD FS, par conséquent, une seule instance des services AD FS est capable d’authentifier et autoriser l’accès pour les utilisateurs qui sont stockées dans les deux AD et non-AD répertoires.

Seule l’authentification basée sur les formulaires est pris en charge pour l’authentification des utilisateurs des annuaires LDAP. L’authentification Windows intégrée et basée sur certificat ne sont pas pris en charge pour l’authentification des utilisateurs dans les annuaires LDAP.

Tous les protocoles d’autorisation passif qui sont pris en charge par AD FS, notamment SAML, WS-Federation et OAuth sont également pris en charge pour les identités qui sont stockées dans les annuaires LDAP.

Le protocole d’autorisation active WS-Trust est également pris en charge pour les identités qui sont stockées dans les annuaires LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurer AD FS pour authentifier les utilisateurs stockés dans un annuaire LDAP
Pour configurer votre batterie AD FS pour authentifier les utilisateurs à partir d’un annuaire LDAP, vous pouvez effectuer les étapes suivantes :

1.  Tout d’abord, configurez une connexion à votre annuaire LDAP à l’aide du **New-AdfsLdapServerConnection** applet de commande :

    ```
    $DirectoryCred = Get-Credential
    $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
    ```

    > [!NOTE]
    > Il est recommandé de créer un nouvel objet de connexion pour chaque serveur LDAP que vous souhaitez vous connecter. AD FS peut se connecter à plusieurs serveurs LDAP de réplica et effectuer un basculement automatique au cas où un serveur LDAP spécifique est arrêté. Pour ce cas, vous pouvez créer des un AdfsLdapServerConnection pour chacun de ces serveurs LDAP de réplica et ajouter ensuite le tableau d’objets de connexion à l’aide de-**LdapServerConnection** paramètre de la  **AdfsLocalClaimsProviderTrust ajouter** applet de commande.

    **REMARQUE :** Votre tentative d’utilisation de Get-Credential et tapez un nom unique et le mot de passe à utiliser pour lier à une instance LDAP peut entraîner un échec car la de la nécessité d’interface utilisateur pour les formats d’entrée spécifiques, par exemple, DOMAINE\nom d’utilisateur ou user@domain.tld. Vous pouvez utiliser à la place de l’applet de commande ConvertTo-SecureString comme suit (l’exemple ci-dessous suppose qu’uid = admin, UO = système en tant que le nom unique des informations d’identification à utiliser pour lier à l’instance LDAP) :

    ```
    $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
    $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
    ```

    Puis entrez le mot de passe pour l’uid = admin et complétez le reste des étapes.

2.  Ensuite, vous pouvez effectuer l’étape facultative de mappage d’attributs LDAP aux revendications AD FS existantes à l’aide de la **New-AdfsLdapAttributeToClaimMapping** applet de commande. Dans l’exemple ci-dessous, vous mappez givenName, Surname, et CommonName LDAP des attributs aux revendications AD FS :

    ```
    #Map given name claim
    $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
    # Map surname claim
    $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
    # Map common name claim
    $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
    ```

    Ce mappage est effectué afin de rendre les attributs à partir du magasin LDAP disponibles en tant que revendications dans ADFS afin de créer des règles de contrôle d’accès conditionnel dans AD FS. Il permet également d’AD FS travailler avec des schémas personnalisés dans les magasins LDAP en fournissant un moyen simple pour mapper les attributs LDAP en cas de réclamation.

3.  Enfin, vous devez inscrire le magasin LDAP avec AD FS comme une approbation de fournisseur à l’aide de revendications le **Add-AdfsLocalClaimsProviderTrust** applet de commande :

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

    Dans l’exemple ci-dessus, vous créez une approbation de fournisseur de revendications local appelée « Fournisseurs ». Vous spécifiez des informations de connexion pour AD FS pour se connecter à l’annuaire LDAP cette approbation de fournisseur de revendications local représente en assignant `$vendorDirectory` à la `-LdapServerConnection` paramètre. Notez qu’à l’étape 1, vous avez affecté `$vendorDirectory` une chaîne de connexion à utiliser lors de la connexion à votre annuaire LDAP spécifique. Enfin, vous spécifiez que le `$GivenName`, `$Surname`, et `$CommonName` les attributs LDAP (ce qui vous avez mappé aux revendications AD FS) doivent être utilisées pour le contrôle d’accès conditionnel, y compris les stratégies d’authentification multifacteur et l’émission règles d’autorisation, ainsi que pour d’émission via des revendications AD FS émis jetons de sécurité. Pour utiliser des protocoles actifs tels que Ws-Trust avec AD FS, vous devez spécifier le paramètre OrganizationalAccountSuffix, qui permet à AD FS lever l’ambiguïté entre les approbations de fournisseur de revendications local lors de la maintenance d’une demande d’autorisation active.

## <a name="see-also"></a>Voir aussi
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)


