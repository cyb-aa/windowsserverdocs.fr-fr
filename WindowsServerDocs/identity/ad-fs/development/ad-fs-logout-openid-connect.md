---
title: Déconnexion unique pour OpenID Connect avec AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fe176af74ebabb5cb56d8aa74d755c4e35ec94a3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857312"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>Déconnexion unique pour OpenID Connect avec AD FS

## <a name="overview"></a>Overview
En s’appuyant sur la prise en charge d’OAuth initiale dans AD FS dans Windows Server 2012 R2, AD FS 2016 a introduit la prise en charge de l’authentification OpenId Connect. Avec [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801), AD FS 2016 prend désormais en charge la déconnexion unique pour les scénarios OpenID Connect. Cet article fournit une vue d’ensemble du scénario de déconnexion unique pour OpenId Connect et fournit des conseils sur la façon de l’utiliser pour vos applications OpenId Connect dans AD FS.


## <a name="discovery-doc"></a>Document de découverte
OpenID Connect utilise un document JSON appelé « document de découverte » pour fournir des détails sur la configuration.  Cela comprend les URI des points de terminaison d’authentification, de jeton, UserInfo et public.  Voici un exemple de document de découverte.

```
{
"issuer":"https://fs.fabidentity.com/adfs",
"authorization_endpoint":"https://fs.fabidentity.com/adfs/oauth2/authorize/",
"token_endpoint":"https://fs.fabidentity.com/adfs/oauth2/token/",
"jwks_uri":"https://fs.fabidentity.com/adfs/discovery/keys",
"token_endpoint_auth_methods_supported":["client_secret_post","client_secret_basic","private_key_jwt","windows_client_authentication"],
"response_types_supported":["code","id_token","code id_token","id_token token","code token","code id_token token"],
"response_modes_supported":["query","fragment","form_post"],
"grant_types_supported":["authorization_code","refresh_token","client_credentials","urn:ietf:params:oauth:grant-type:jwt-bearer","implicit","password","srv_challenge"],
"subject_types_supported":["pairwise"],
"scopes_supported":["allatclaims","email","user_impersonation","logon_cert","aza","profile","vpn_cert","winhello_cert","openid"],
"id_token_signing_alg_values_supported":["RS256"],
"token_endpoint_auth_signing_alg_values_supported":["RS256"],
"access_token_issuer":"http://fs.fabidentity.com/adfs/services/trust",
"claims_supported":["aud","iss","iat","exp","auth_time","nonce","at_hash","c_hash","sub","upn","unique_name","pwd_url","pwd_exp","sid"],
"microsoft_multi_refresh_token":true,
"userinfo_endpoint":"https://fs.fabidentity.com/adfs/userinfo",
"capabilities":[],
"end_session_endpoint":"https://fs.fabidentity.com/adfs/oauth2/logout",
"as_access_token_token_binding_supported":true,
"as_refresh_token_token_binding_supported":true,
"resource_access_token_token_binding_supported":true,
"op_id_token_token_binding_supported":true,
"rp_id_token_token_binding_supported":true,
"frontchannel_logout_supported":true,
"frontchannel_logout_session_supported":true
} 
 
```



Les valeurs supplémentaires suivantes seront disponibles dans le document de découverte pour indiquer la prise en charge de la déconnexion du canal frontal :

- frontchannel_logout_supported : la valeur est « true »
- frontchannel_logout_session_supported : la valeur sera « true ».
- end_session_endpoint : il s’agit de l’URI de déconnexion OAuth que le client peut utiliser pour lancer la déconnexion sur le serveur.


## <a name="ad-fs-server-configuration"></a>Configuration du serveur AD FS
La propriété AD FS EnableOAuthLogout est activée par défaut.  Cette propriété indique au serveur AD FS de Rechercher l’URL (LogoutURI) avec le SID pour lancer la déconnexion sur le client. Si [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) n’est pas installé, vous pouvez utiliser la commande PowerShell suivante :

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` paramètre sera marqué comme obsolète après l’installation de [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801). `EnableOAUthLogout` est toujours true et n’aura aucun impact sur la fonctionnalité de déconnexion.

>[!NOTE]
>frontchannel_logout est pris en charge **uniquement** après installation vcredist de [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Configuration client
Le client doit implémenter une URL désignant l’utilisateur connecté. L’administrateur peut configurer LogoutUri dans la configuration du client à l’aide des applets de commande PowerShell suivantes. 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

Le `LogoutUri` est l’URL utilisée par AF FS pour « fermer la session » de l’utilisateur. Pour implémenter l' `LogoutUri`, le client doit s’assurer qu’il efface l’état d’authentification de l’utilisateur dans l’application, par exemple, en supprimant les jetons d’authentification dont il dispose. AD FS accédera à cette URL, avec le SID comme paramètre de requête, en signalant à la partie de confiance/application qu’elle doit se déconnecter de l’utilisateur. 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **Jeton OAuth avec l’ID de session**: AD FS comprend l’ID de session dans le jeton OAuth au moment de l’émission du jeton id_token. Ce sera utilisé ultérieurement par AD FS pour identifier les cookies SSO appropriés à nettoyer pour l’utilisateur.
2.  L' **utilisateur lance la déconnexion sur App1**: l’utilisateur peut lancer une déconnexion à partir de n’importe quelle application connectée. Dans cet exemple de scénario, un utilisateur lance une déconnexion à partir de App1.
3.  L' **application envoie une demande de déconnexion à AD FS**: une fois que l’utilisateur a lancé la déconnexion, l’application envoie une requête d’extraction à end_session_endpoint de AD FS. L’application peut éventuellement inclure id_token_hint en tant que paramètre de cette requête. Si id_token_hint est présent, AD FS l’utilise conjointement avec l’ID de session pour déterminer l’URI vers lequel le client doit être redirigé après la déconnexion (post_logout_redirect_uri).  Le post_logout_redirect_uri doit être un URI valide inscrit avec AD FS à l’aide du paramètre RedirectUris.
4.  **AD FS envoie la déconnexion aux clients connectés**: AD FS utilise la valeur de l’identificateur de session pour rechercher les clients appropriés auxquels l’utilisateur est connecté. Les clients identifiés reçoivent une demande sur le LogoutUri inscrit auprès de AD FS pour initier une déconnexion côté client.

## <a name="faqs"></a>FAQ
**Q :** Je ne vois pas les paramètres frontchannel_logout_supported et frontchannel_logout_session_supported dans le document de découverte.</br>
**R :** Assurez-vous que [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) est installé sur tous les serveurs AD FS. Reportez-vous à la déconnexion unique dans le serveur 2016 avec [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801).

**Q :** J’ai configuré Single Logout comme étant dirigée, mais l’utilisateur reste connecté sur d’autres clients.</br>
**R :** Assurez-vous que `LogoutUri` est défini pour tous les clients où l’utilisateur est connecté. De plus, AD FS tente d’envoyer la demande de déconnexion sur le `LogoutUri`inscrit. Le client doit implémenter la logique pour gérer la demande et prendre une mesure pour déconnecter l’utilisateur de l’application.</br>

**Q :** Si, après la déconnexion, l’un des clients revient à AD FS avec un jeton d’actualisation valide, AD FS émettre un jeton d’accès ?</br>
**R :** Oui. Il incombe à l’application cliente de supprimer tous les artefacts authentifiés après la réception d’une demande de déconnexion au `LogoutUri`inscrit.


## <a name="next-steps"></a>Étapes suivantes
[Développement des services AD FS](../../ad-fs/AD-FS-Development.md)  
