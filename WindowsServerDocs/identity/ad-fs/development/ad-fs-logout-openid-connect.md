---
title: Déconnexion unique pour OpenID Connect avec AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3af10ec139edbc72e75bf80f544ac5b4f1cf9222
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825770"
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>Déconnexion unique pour OpenID Connect avec AD FS

## <a name="overview"></a>Vue d'ensemble
La prise en charge Oauth initiale dans AD FS dans Windows Server 2012 R2, AD FS 2016 introduit la prise en charge pour l’authentification OpenId Connect. Avec [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801), AD FS 2016 prend en charge la déconnexion unique pour les scénarios OpenId Connect. Cet article fournit une vue d’ensemble de la déconnexion unique pour le scénario OpenId Connect et fournit des conseils sur son utilisation pour vos applications OpenId Connect dans AD FS.


## <a name="discovery-doc"></a>Document de découverte
OpenID Connect utilise un document JSON appelé un « document de découverte » pour fournir des détails sur la configuration.  Cela inclut les URI de l’authentification, le jeton, l’userinfo et le public-points de terminaison.  Voici un exemple de la documentation de découverte.

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



Les valeurs supplémentaires suivantes seront disponibles dans le document de découverte pour indiquer la prise en charge de la déconnexion de canal avant :

- frontchannel_logout_supported : valeur sera 'true'
- frontchannel_logout_session_supported : valeur sera 'true'.
- end_session_endpoint : c’est l’URI que le client peut utiliser pour initier la déconnexion sur le serveur de la déconnexion OAuth.


## <a name="ad-fs-server-configuration"></a>Configuration du serveur AD FS
La propriété AD FS EnableOAuthLogout sera activée par défaut.  Cette propriété indique le serveur AD FS pour accéder à l’URL (LogoutURI) avec le SID pour initier la déconnexion sur le client. Si vous n’avez pas [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) installée, vous pouvez utiliser la commande PowerShell suivante :

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` paramètre sera marqué comme obsolète après l’installation de [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801). `EnableOAUthLogout` sera toujours true et n’a aucun impact sur la fonctionnalité de déconnexion.

>[!NOTE]
>la prise en charge de frontchannel_logout **uniquement** après l’installation de [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Configuration du client
Client doit implémenter une url qui « se déconnecte' connecté dans l’utilisateur. Administrateur peut configurer le LogoutUri dans la configuration du client à l’aide des applets de commande PowerShell suivante. 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

Le `LogoutUri` est l’url utilisée par AF FS « déconnecter » l’utilisateur. Pour implémenter le `LogoutUri`, le client a besoin pour garantir qu’il efface l’état d’authentification de l’utilisateur dans l’application, par exemple, la suppression de l’authentification des jetons qu’il possède. AD FS sera accédez à cette URL, avec le SID que le paramètre de requête, la partie de confiance de signalisation / application pour vous déconnecter de l’utilisateur. 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **Jeton OAuth avec l’ID de session**: AD FS inclut un id de session dans le jeton OAuth au moment de l’émission du jeton id_token. Il servira ultérieurement par AD FS pour identifier les cookies SSO pertinentes soient nettoyées pour l’utilisateur.
2.  **Utilisateur initie la déconnexion sur App1**: L’utilisateur peut initier une déconnexion de toutes les applications connectées. Dans cet exemple de scénario, un utilisateur lance une déconnexion de App1.
3.  **Application envoie la demande de déconnexion à AD FS**: Une fois que l’utilisateur lance la déconnexion, l’application envoie une requête GET à end_session_endpoint d’AD FS. L’application peut éventuellement inclure id_token_hint en tant que paramètre à cette demande. Si id_token_hint est présent, AD FS sera utilisez-le conjointement avec l’ID de session pour figure out le URI, le client doit être redirigé vers après la déconnexion (post_logout_redirect_uri).  Le post_logout_redirect_uri doit être un uri valide est inscrit avec AD FS en utilisant le paramètre RedirectUris.
4.  **AD FS envoie déconnexion aux clients connectés**: AD FS utilise la valeur d’identificateur de session pour rechercher les clients pertinents, à que l’utilisateur est connecté. Les clients identifiés sont envoyés de demande sur le LogoutUri inscrit avec AD FS pour initier une déconnexion du côté client.

## <a name="faqs"></a>FAQ
**Q :** Je ne vois pas les paramètres frontchannel_logout_supported et frontchannel_logout_session_supported dans le document de découverte.</br>
**R :** Assurez-vous d’avoir [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) installé sur tous les serveurs AD FS. Reportez-vous à la déconnexion unique dans Server 2016 avec [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801).

**Q :** J’ai configuré la déconnexion unique comme indiqué, mais utilisateur reste connecté en sur d’autres clients.</br>
**R :** Vérifiez que `LogoutUri` est définie pour tous les clients où l’utilisateur est connecté. En outre, AD FS effectue une tentative d’idéale pour envoyer la demande de déconnexion sur inscrit `LogoutUri`. Client doit implémenter la logique pour traiter la demande et d’effectuer une opération de déconnexion l’utilisateur à partir de l’application.</br>

**Q :** Si après la déconnexion, un des clients revient à AD FS avec un jeton d’actualisation valide, ADFS émet un jeton d’accès ?</br>
**R :** Oui. Il incombe à l’application cliente pour supprimer les artefacts tout authentifiés après une demande de déconnexion a été reçue à inscrit `LogoutUri`.


## <a name="next-steps"></a>Étapes suivantes
[Développement de AD FS](../../ad-fs/AD-FS-Development.md)  
