---
title: Résolution des problèmes d’AD FS - Azure AD
description: Ce document explique comment résoudre les divers aspects d’AD FS et Azure AD
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 228ef34ab25276c1cf98f9b2b64e997390023c87
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444009"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>Résolution des problèmes d’AD FS - Azure AD
Avec l’essor du cloud, un grand nombre d’entreprises ont choisi d’utiliser Azure AD pour leurs applications et services divers.  Fédération avec Azure AD sont devenus une pratique standard avec de nombreuses organisations.  Ce document aborde certains aspects de résolution des problèmes qui surviennent avec cette fédération.  Plusieurs des rubriques dans le document de dépannage général se rapportent toujours à la fédération avec Azure afin de ce document se concentrera sur les spécificités juste avec Azure AD et l’interaction d’AD FS.

## <a name="redirection-to-ad-fs"></a>Redirection vers AD FS
La redirection produit lorsque vous êtes connecté à une application, tels que Office 365 et vous êtes « redirigé » à votre organisation les serveurs AD FS pour se connecter.

![](media/ad-fs-tshoot-azure/azure1.png)


### <a name="first-things-to-check"></a>Premières choses à vérifier
Si la redirection n’a pas eu lieu qu'il y a quelques éléments que vous souhaitez vérifier

   1. Vérifiez que votre client Azure AD est activé pour la fédération en vous connectant au portail Azure et la vérification sous Azure AD Connect.

![](media/ad-fs-tshoot-azure/azure2.png)

1. Assurez-vous que votre domaine personnalisé est vérifié en cliquant sur le domaine en regard de la fédération dans le portail Azure.
   ![](media/ad-fs-tshoot-azure/azure3.png)

2. Enfin, vous souhaitez vérifier [DNS](ad-fs-tshoot-dns.md) et assurez-vous que vos serveurs AD FS ou les serveurs WAP sont résout à partir d’internet.  Vérifiez que cela résout et que vous êtes en mesure d’y accéder.
3. Vous pouvez également utiliser l’applet de commande PowerShell `Get-AzureADDomain` pour obtenir ces informations également.

![](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>Vous recevez une erreur de méthode d’authentification inconnu
Vous pouvez rencontrer une erreur « Méthode d’authentification inconnu » indiquant que la AuthnContext n’est pas pris en charge au niveau AD FS ou STS lorsque vous êtes redirigé à partir d’Azure. 

Cela est plus courant lorsqu’Azure AD redirige vers AD FS ou STS à l’aide d’un paramètre qui applique une méthode d’authentification. 

Pour appliquer une méthode d’authentification, utilisez une des méthodes suivantes :
- Pour WS-Federation, utilisez une chaîne de requête WAUTH pour forcer une méthode d’authentification recommandée.

- Pour SAML2.0, utilisez ce qui suit :
  ```
  <saml:AuthnContext>
  <saml:AuthnContextClassRef>
  urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
  </saml:AuthnContextClassRef>
  </saml:AuthnContext>
  ```
  Lorsque la méthode d’authentification appliquée est envoyée avec une valeur incorrecte, ou si cette méthode d’authentification n’est pas pris en charge sur AD FS ou STS, vous recevez un message d’erreur avant que vous êtes authentifié.

|Méthode d’authentification voulue|wauth URI|
|-----|-----|
|Authentification par nom d’utilisateur et mot de passe|urn : oasis : noms : tc : SAML:1.0:am:password|
|Authentification du client SSL|urn:ietf:rfc:2246|
|Authentification intégrée de Windows|urn:federation:authentication:windows|

Classes de contexte de l’authentification SAML pris en charge

|Méthodes d'authentification|Classe de contexte d’authentification URI|
|-----|-----| 
|Nom d’utilisateur et mot de passe|urn : oasis : noms : tc : SAML:2.0:ac:classes:Password|
|Transport de mot de passe protégé|urn : oasis : noms : tc : SAML:2.0:ac:classes:PasswordProtectedTransport|
|Layer Security (TLS) de client de transport|urn : oasis : noms : tc : SAML:2.0:ac:classes:TLSClient
|Certificat X.509|urn : oasis : noms : tc : SAML:2.0:ac:classes:X 509
|Authentification Windows intégrée|urn:federation:authentication:windows|
|Kerberos|urn : oasis : noms : tc : SAML:2.0:ac:classes:Kerberos|

Pour vous assurer que la méthode d’authentification est pris en charge au niveau AD FS, vérifiez les points suivants.

#### <a name="ad-fs-20"></a>AD FS 2.0 

Sous **/adfs/ls/web.config**, assurez-vous que l’entrée pour le type d’authentification est présente.

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

Sous **gestion AD FS**, cliquez sur **stratégies d’authentification** dans les services AD FS-composant logiciel enfichable.

Dans le **l’authentification principale** , cliquez sur Modifier en regard des paramètres globaux. Vous pouvez également cliquez sur stratégies d’authentification, puis sélectionnez Modifier l’authentification principale globale. Ou, dans le volet Actions, sélectionnez Modifier l’authentification principale globale.

Dans la fenêtre Modifier la stratégie d’authentification globale, sous l’onglet principal, vous pouvez configurer des paramètres dans le cadre de la stratégie d’authentification globale. Par exemple, pour l’authentification principale, vous pouvez sélectionner les méthodes d’authentification disponibles sous Intranet et Extranet.

** Vérifiez que l’option la méthode d’authentification requise case à cocher est sélectionnée. 

#### <a name="ad-fs-2016"></a>AD FS 2016

Sous **gestion AD FS**, cliquez sur **Service** et **méthodes d’authentification** dans les services AD FS-composant logiciel enfichable.

Dans le **l’authentification principale** section, cliquez sur Modifier.

Dans le **modifier les méthodes d’authentification** fenêtre, sous l’onglet principal, vous pouvez configurer des paramètres dans le cadre de la stratégie d’authentification.

![](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>Jetons émis par AD FS

### <a name="azure-ad-throws-error-after-token-issuance"></a>Azure AD génère l’erreur après l’émission de jeton
Une fois que AD FS émet un jeton, Azure AD peut lever une erreur. Dans ce cas, recherchez les problèmes suivants :
- Les revendications qui sont émises par AD FS dans le jeton doivent correspondent aux attributs respectifs de l’utilisateur dans Azure AD.
- le jeton pour Azure AD doit contenir les revendications requises suivantes :
    - WS-FED : 
        - UPN : La valeur de cette revendication doit correspondre à l’UPN des utilisateurs dans Azure AD.
        - ImmutableID : La valeur de cette revendication doit correspondre à l’attribut sourceAnchor ou ImmutableID de l’utilisateur dans Azure AD.

Pour obtenir la valeur d’attribut utilisateur dans Azure AD, exécutez la ligne de commande suivante : `Get-AzureADUser –UserPrincipalName <UPN>`

![](media/ad-fs-tshoot-azure/azure5.png)

   - SAML 2.0 :
       - IDPEmail : La valeur de cette revendication doit correspondre le nom d’utilisateur principal des utilisateurs dans Azure AD.
       - NAMEID : La valeur de cette revendication doit correspondre à l’attribut sourceAnchor ou ImmutableID de l’utilisateur dans Azure AD.

Pour plus d’informations, consultez [utiliser un fournisseur d’identité SAML 2.0 pour implémenter l’authentification unique sur](https://technet.microsoft.com/library/dn641269.aspx).

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>Incompatibilité du certificat signature de jetons entre AD FS et Azure AD.

AD FS utilise le certificat de signature de jetons pour signer le jeton qui est envoyé à l’utilisateur ou l’application. L’approbation entre les services AD FS et Azure AD est une approbation fédérée a fondé sur ce certificat de signature de jetons.

Toutefois, si le certificat de signature de jetons sur le côté d’AD FS est modifié en raison de la substitution de certificat automatique ou par une intervention, les détails du nouveau certificat doivent être mis à jour sur le côté Azure AD pour le domaine fédéré. Lorsque le certificat de signature de jetons principal sur les services AD FS diffère des annuaires Azure AD, le jeton émis par AD FS n’est pas approuvé par Azure AD. Par conséquent, l’utilisateur fédéré n’est pas autorisé à ouvrir une session.

Pour corriger le problème que vous pouvez utiliser les différentes étapes dans [renouveler les certificats de fédération pour Office 365 et Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs).

## <a name="other-common-things-to-check"></a>Autres points à vérifier
Voici une liste rapide des éléments à vérifier si vous rencontrez des problèmes avec une interaction AD FS et Azure AD.
- informations d’identification obsolètes ou mis en cache dans le Gestionnaire d’informations d’identification Windows
- Algorithme de hachage sécurisé qui est configuré sur la confiance pour Office 365 est défini sur SHA1

## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)