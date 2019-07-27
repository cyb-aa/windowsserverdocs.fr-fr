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
ms.openlocfilehash: 87ada412e6b4ab47aa18a62953b84d8a1369dffa
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544519"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>Configuration d'AD FS pour authentifier les utilisateurs stockées dans des annuaires LDAP

La rubrique suivante décrit la configuration requise pour permettre à votre infrastructure AD FS d’authentifier les utilisateurs dont les identités sont stockées dans des annuaires compatibles LDAP (Lightweight Directory Access Protocol) v3.

Dans de nombreuses organisations, les solutions de gestion des identités consistent en une combinaison de Active Directory, AD LDS ou des annuaires LDAP tiers. Avec l’ajout de la prise en charge de AD FS pour l’authentification des utilisateurs stockés dans des annuaires compatibles LDAP v3, vous pouvez tirer parti de l’ensemble des fonctionnalités de AD FS d’entreprise, quel que soit l’emplacement de stockage de vos identités d’utilisateur. AD FS prend en charge tous les répertoires compatibles LDAP v3.

> [!NOTE]
> Parmi les fonctionnalités AD FSes, citons l’authentification unique (SSO), l’authentification des appareils, les stratégies d’accès conditionnel flexibles, la prise en charge de l’utilisation à partir de n’importe où via l’intégration au proxy d’application Web et la Fédération transparente avec Azure AD qui, à son tour, permet à vous et à vos utilisateurs d’utiliser le Cloud, notamment Office 365 et d’autres applications SaaS.  Pour plus d’informations, consultez [services ADFS vue d’ensemble](../../ad-fs/AD-FS-2016-Overview.md).

Pour que les AD FS authentifient les utilisateurs à partir d’un annuaire LDAP, vous devez connecter cet annuaire LDAP à votre batterie de serveurs AD FS en créant une **approbation de fournisseur de revendications locale**.  Une approbation de fournisseur de revendications locale est un objet d’approbation qui représente un répertoire LDAP dans votre batterie de AD FS. Un objet d’approbation de fournisseur de revendications local se compose d’un ensemble d’identificateurs, de noms et de règles qui identifient cet annuaire LDAP auprès du service de Fédération local.

Vous pouvez prendre en charge plusieurs annuaires LDAP, chacun avec sa propre configuration, au sein d’une même batterie de serveurs AD FS en ajoutant plusieurs approbations de **fournisseur de revendications locales**. En outre, AD DS forêts qui ne sont pas approuvées par la forêt dans laquelle AD FS vit peuvent également être modélisées en tant qu’approbations de fournisseur de revendications locales. Vous pouvez créer des approbations de fournisseur de revendications locales à l’aide de Windows PowerShell.

Les annuaires LDAP (approbations de fournisseur de revendications locales) peuvent coexister avec des répertoires AD (approbations de fournisseur de revendications) sur le même serveur de AD FS, au sein de la même batterie de AD FS, par conséquent, une seule instance de AD FS est capable d’authentifier et d’autoriser l’accès pour les utilisateurs qui sont stockées dans les annuaires AD et non AD.

Seule l’authentification basée sur les formulaires est prise en charge pour l’authentification des utilisateurs dans les annuaires LDAP. L’authentification Windows intégrée et basée sur les certificats n’est pas prise en charge pour l’authentification des utilisateurs dans les annuaires LDAP.

Tous les protocoles d’autorisation passifs pris en charge par AD FS, notamment SAML, WS-Federation et OAuth, sont également pris en charge pour les identités stockées dans les annuaires LDAP.

Le protocole WS-Trust active Authorization est également pris en charge pour les identités stockées dans des annuaires LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurer AD FS pour authentifier les utilisateurs stockés dans un annuaire LDAP
Pour configurer votre batterie de AD FS afin d’authentifier les utilisateurs à partir d’un annuaire LDAP, vous pouvez effectuer les étapes suivantes:

1. Tout d’abord, configurez une connexion à votre annuaire LDAP à l’aide de l’applet **de commande New-AdfsLdapServerConnection** :

   ```
   $DirectoryCred = Get-Credential
   $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
   ```

   > [!NOTE]
   > Il est recommandé de créer un nouvel objet de connexion pour chaque serveur LDAP auquel vous souhaitez vous connecter. AD FS peut se connecter à plusieurs serveurs LDAP de réplication et effectuer un basculement automatique en cas de panne d’un serveur LDAP spécifique. Dans ce cas, vous pouvez créer un AdfsLdapServerConnection pour chacun de ces serveurs LDAP de réplication, puis ajouter le tableau d’objets de connexion à l’aide du paramètre-**LdapServerConnection** de l’applet de commande **Add-AdfsLocalClaimsProviderTrust** .

   **REMARQUE :** Votre tentative d’utilisation de la classe de connexion et du type dans un nom de domaine et un mot de passe à utiliser pour établir une liaison à une instance LDAP peut entraîner un échec en raison de l’exigence de l’interface user@domain.tldutilisateur pour des formats d’entrée spécifiques, par exemple domaine\nom_utilisateur ou. Vous pouvez à la place utiliser l’applet de commande ConvertTo-SecureString comme suit (l’exemple ci-dessous suppose que uid = admin, ou = System comme DN des informations d’identification à utiliser pour la liaison à l’instance LDAP):

   ```
   $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
   $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
   ```

   Entrez ensuite le mot de passe pour UID = admin et suivez les étapes restantes.

2. Ensuite, vous pouvez effectuer l’étape facultative de mappage des attributs LDAP aux revendications de AD FS existantes à l’aide de l’applet **de commande New-AdfsLdapAttributeToClaimMapping** . Dans l’exemple ci-dessous, vous mappez les attributs de nom de famille givenName, Surname et CommonName au AD FS revendications:

   ```
   #Map given name claim
   $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
   # Map surname claim
   $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
   # Map common name claim
   $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
   ```

   Ce mappage est effectué afin de rendre disponibles les attributs du magasin LDAP en tant que revendications dans AD FS afin de créer des règles de contrôle d’accès conditionnel dans AD FS. Il permet également à AD FS d’utiliser des schémas personnalisés dans des magasins LDAP en fournissant un moyen simple de mapper les attributs LDAP aux revendications.

3. Enfin, vous devez inscrire le magasin LDAP avec AD FS comme approbation de fournisseur de revendications local à l’aide de l’applet de commande **Add-AdfsLocalClaimsProviderTrust** :

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

   Dans l’exemple ci-dessus, vous créez une approbation de fournisseur de revendications locale appelée «Vendors». Vous spécifiez des informations de connexion pour AD FS pour vous connecter au répertoire LDAP que cette approbation de fournisseur de revendications `$vendorDirectory` locale représente `-LdapServerConnection` en affectant au paramètre. Notez que à l’étape 1, vous avez `$vendorDirectory` affecté une chaîne de connexion à utiliser lors de la connexion à votre annuaire LDAP spécifique. Enfin, vous spécifiez que les `$GivenName` `$Surname`attributs LDAP, `$CommonName` et (que vous avez mappés aux revendications AD FS) doivent être utilisés pour le contrôle d’accès conditionnel, y compris les stratégies d’authentification multifacteur et l’émission. règles d’autorisation, ainsi que pour l’émission via des revendications dans des jetons de sécurité émis par AD FS. Pour pouvoir utiliser des protocoles actifs tels que WS-Trust avec AD FS, vous devez spécifier le paramètre OrganizationalAccountSuffix, qui permet à AD FS de lever l’ambiguïté entre les approbations de fournisseur de revendications locales lors du traitement d’une demande d’autorisation active.

## <a name="see-also"></a>Voir aussi
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)


