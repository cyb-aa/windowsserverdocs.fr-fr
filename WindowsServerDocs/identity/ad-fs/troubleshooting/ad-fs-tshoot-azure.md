---
title: Résolution des problèmes de AD FS-Azure AD
description: Ce document explique comment dépanner divers aspects de AD FS et Azure AD
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 293618b3fe2a24caff8fd6b52c5528cc699f93de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407281"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>Résolution des problèmes de AD FS-Azure AD
Avec la croissance du Cloud, beaucoup d’entreprises ont été déplacées pour utiliser Azure AD pour leurs applications et services divers.  La Fédération avec Azure AD est devenue une pratique standard avec de nombreuses organisations.  Ce document aborde certains aspects de la résolution des problèmes qui surviennent avec cette fédération.  Plusieurs des rubriques du document de résolution des problèmes généraux se rapportent toujours à la Fédération avec Azure. ce document se concentre donc sur les spécificités de Azure AD et AD FS interaction.

## <a name="redirection-to-ad-fs"></a>Redirection vers AD FS
La redirection se produit lorsque vous vous connectez à une application telle que Office 365 et que vous êtes « redirigé » vers votre organisation AD FS serveurs pour la connexion.

![](media/ad-fs-tshoot-azure/azure1.png)


### <a name="first-things-to-check"></a>Premiers éléments à vérifier
Si la redirection n’a pas lieu, vous devez vérifier quelques éléments

   1. Assurez-vous que votre locataire Azure AD est activé pour la Fédération en vous connectant au Portail Azure et en vérifiant sous Azure AD Connect.

![](media/ad-fs-tshoot-azure/azure2.png)

1. Vérifiez que votre domaine personnalisé est vérifié en cliquant sur le domaine en regard de Fédération dans la Portail Azure.
   ![](media/ad-fs-tshoot-azure/azure3.png)

2. Enfin, vous souhaitez vérifier [DNS](ad-fs-tshoot-dns.md) et vous assurer que vos serveurs AD FS ou vos serveurs WAP se résolvent à partir d’Internet.  Vérifiez que cette valeur est résolue et que vous pouvez y accéder.
3. Vous pouvez également utiliser le `Get-AzureADDomain` PowerShell applet pour récupérer ces informations.

![](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>Vous recevez une erreur de méthode d’authentification inconnue
Vous pouvez rencontrer une erreur « méthode d’authentification inconnue » indiquant que AuthnContext n’est pas pris en charge au niveau du AD FS ou STS lorsque vous êtes redirigé à partir d’Azure. 

Cela se passe le plus souvent lorsque Azure AD redirige vers le AD FS ou STS à l’aide d’un paramètre qui applique une méthode d’authentification. 

Pour appliquer une méthode d’authentification, utilisez l’une des méthodes suivantes :
- Pour WS-Federation, utilisez une chaîne de requête WAUTH pour forcer une méthode d’authentification par défaut.

- Pour SAML 2.0, utilisez ce qui suit :
  ```
  <saml:AuthnContext>
  <saml:AuthnContextClassRef>
  urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
  </saml:AuthnContextClassRef>
  </saml:AuthnContext>
  ```
  Lorsque la méthode d’authentification appliquée est envoyée avec une valeur incorrecte, ou si cette méthode d’authentification n’est pas prise en charge sur AD FS ou STS, vous recevez un message d’erreur avant d’être authentifié.

|Méthode d’authentification souhaitée|URI wauth|
|-----|-----|
|Authentification par nom d’utilisateur et mot de passe|urn : Oasis : Names : TC : SAML : 1.0 : AM : Password|
|Authentification du client SSL|urn:ietf:rfc:2246|
|Authentification intégrée de Windows|urn : Federation : authentification : Windows|

Classes de contexte d’authentification SAML prises en charge

|Méthodes d'authentification|URI de classe de contexte d’authentification|
|-----|-----| 
|Nom d’utilisateur et mot de passe|urn : Oasis : Names : TC : SAML : 2.0 : AC : classes : Password|
|Transport protégé par mot de passe|urn : Oasis : Names : TC : SAML : 2.0 : AC : classes : PasswordProtectedTransport|
|Client TLS (Transport Layer Security)|urn : Oasis : Names : TC : SAML : 2.0 : AC : classes : par formulaires
|Certificat X. 509|urn : Oasis : Names : TC : SAML : 2.0 : AC : classes : x509
|Authentification Windows intégrée|urn : Federation : authentification : Windows|
|Kerberos|urn : Oasis : Names : TC : SAML : 2.0 : AC : classes : Kerberos|

Pour vous assurer que la méthode d’authentification est prise en charge au niveau de la AD FS, vérifiez les points suivants.

#### <a name="ad-fs-20"></a>AD FS 2,0 

Sous **/ADFS/LS/Web.config**, assurez-vous que l’entrée correspondant au type d’authentification est présente.

```
<microsoft.identityServer.web>
<localAuthenticationTypes>
<add name="Forms" page="FormsSignIn.aspx" />
<add name="Integrated" page="auth/integrated/" />
<add name="TlsClient" page="auth/sslclient/" />
<add name="Basic" page="auth/basic/" />
</localAuthenticationTypes>
```

#### <a name="ad-fs-2012-r2"></a>AD FS 2012 R2

Sous **gestion des AD FS**, cliquez sur **stratégies d’authentification** dans le composant logiciel enfichable AD FS.

Dans la section **authentification principale** , cliquez sur modifier en regard de paramètres globaux. Vous pouvez également cliquer avec le bouton droit sur stratégies d’authentification, puis sélectionner modifier l’authentification principale globale. Ou, dans le volet Actions, sélectionnez Modifier l’authentification principale globale.

Dans la fenêtre Modifier la stratégie d’authentification globale, sous l’onglet principal, vous pouvez configurer les paramètres dans le cadre de la stratégie d’authentification globale. Par exemple, pour l’authentification principale, vous pouvez sélectionner les méthodes d’authentification disponibles sous extranet et intranet.

\* * Assurez-vous que la case à cocher méthode d’authentification requise est activée. 

#### <a name="ad-fs-2016"></a>AD FS 2016

Sous **gestion des AD FS**, cliquez sur **service** et **méthodes d’authentification** dans le composant logiciel enfichable AD FS.

Dans la section **authentification principale** , cliquez sur modifier.

Dans la fenêtre **modifier les méthodes d’authentification** , sous l’onglet principal, vous pouvez configurer les paramètres dans le cadre de la stratégie d’authentification.

![](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>Jetons émis par AD FS

### <a name="azure-ad-throws-error-after-token-issuance"></a>Azure AD lève une erreur après l’émission du jeton
Une fois que AD FS émet un jeton, Azure AD peut générer une erreur. Dans ce cas, vérifiez les problèmes suivants :
- Les revendications émises par AD FS dans le jeton doivent correspondre aux attributs respectifs de l’utilisateur dans Azure AD.
- le jeton de Azure AD doit contenir les revendications requises suivantes :
    - WSFED 
        - UPN : la valeur de cette revendication doit correspondre à l’UPN des utilisateurs dans Azure AD.
        - ImmutableID : la valeur de cette revendication doit correspondre au sourceAnchor ou ImmutableID de l’utilisateur dans Azure AD.

Pour récupérer la valeur de l’attribut User dans Azure AD, exécutez la ligne de commande suivante : `Get-AzureADUser –UserPrincipalName <UPN>`

![](media/ad-fs-tshoot-azure/azure5.png)

   - 2,0 SAML :
       - Idpemail il : la valeur de cette revendication doit correspondre au nom d’utilisateur principal des utilisateurs dans Azure AD.
       - NAMEID : la valeur de cette revendication doit correspondre à sourceAnchor ou ImmutableID de l’utilisateur dans Azure AD.

Pour plus d’informations, consultez [utiliser un fournisseur d’identité SAML 2,0 pour implémenter l’authentification unique](https://technet.microsoft.com/library/dn641269.aspx).

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>Incompatibilité de certificat de signature de jetons entre AD FS et Azure AD.

AD FS utilise le certificat de signature de jetons pour signer le jeton envoyé à l’utilisateur ou à l’application. L’approbation entre les AD FS et Azure AD est une approbation fédérée basée sur ce certificat de signature de jetons.

Toutefois, si le certificat de signature de jetons côté AD FS est modifié en raison de la substitution de certificat automatique ou d’une intervention, les détails du nouveau certificat doivent être mis à jour côté Azure AD pour le domaine fédéré. Lorsque le certificat de signature de jetons principal sur le AD FS est différent d’Azure ADs, le jeton émis par AD FS n’est pas approuvé par Azure AD. Par conséquent, l’utilisateur fédéré n’est pas autorisé à se connecter.

Pour résoudre ce problème, vous pouvez suivre les étapes décrites dans [renouveler les certificats de Fédération pour Office 365 et Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).

## <a name="other-common-things-to-check"></a>Autres éléments courants à vérifier
Voici une liste rapide des éléments à vérifier si vous rencontrez des problèmes avec AD FS et Azure AD interaction.
- informations d’identification périmées ou mises en cache dans le gestionnaire d’informations d’identification Windows
- L’algorithme de hachage sécurisé configuré sur l’approbation de la partie de confiance pour Office 365 est défini sur SHA1

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)