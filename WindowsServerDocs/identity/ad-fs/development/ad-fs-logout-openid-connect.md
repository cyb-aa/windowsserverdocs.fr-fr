---
title: "Déconnexion unique pour OpenID Connect avec ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3af10ec139edbc72e75bf80f544ac5b4f1cf9222
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
#  <a name="single-log-out-for-openid-connect-with-ad-fs"></a>Déconnexion unique pour OpenID Connect avec ADFS

## <a name="overview"></a>Vue d’ensemble
La prise en charge Oauth initiale dans ADFS dans Windows Server2012R2, ADFS2016 introduit la prise en charge pour se connecter OpenId ouverture de session. Avec [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801), ADFS2016 maintenant prend en charge les déconnexions unique pour les scénarios OpenId se connecter. Cet article fournit une vue d’ensemble de la déconnexion unique pour le scénario OpenId se connecter et fournit des conseils sur la façon de l’utiliser pour vos applications de se connecter OpenId dans ADFS.


## <a name="discovery-doc"></a>Document de découverte
Se connecter OpenID utilise un document JSON appelé "document découverte" pour fournir des détails sur la configuration.  Cela inclut les URI de l’authentification, jeton, userinfo et public-points de terminaison.  Voici un exemple du document de découverte.

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



Les valeurs supplémentaires suivantes seront disponibles dans le document de découverte pour indiquer la prise en charge de la déconnexion de canal avant:

- frontchannel_logout_supported: valeur est "true"
- frontchannel_logout_session_supported: valeur est "true".
- end_session_endpoint: il s’agit de la déconnexion OAuth URI que le client peut utiliser pour lancer la fermeture de session sur le serveur.


## <a name="ad-fs-server-configuration"></a>Configuration du serveur ADFS
La propriété ADFS EnableOAuthLogout sera activée par défaut.  Cette propriété indique le serveur ADFS pour rechercher l’URL (LogoutURI) avec le SID pour lancer la déconnexion sur le client. Si vous n’avez pas [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) installé, vous pouvez utiliser la commande PowerShell suivante:

```PowerShell
Set-ADFSProperties -EnableOAuthLogout $true
```

>[!NOTE]
> `EnableOAuthLogout` Paramètre sera marqué comme obsolète après l’installation de [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801). `EnableOAUthLogout` Aura toujours la valeur true et n’a aucun impact sur les fonctionnalités de déconnexion.

>[!NOTE]
>frontchannel_logout est pris en charge **uniquement** après l’installation de [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801)

## <a name="client-configuration"></a>Configuration du client
Client doit implémenter une url qui 'se déconnecte' connecté utilisateur. Administrateur peut configurer le LogoutUri dans la configuration du client à l’aide des applets de commande PowerShell suivante. 


- `(Add | Set)-AdfsNativeApplication`
- `(Add | Set)-AdfsServerApplication`
- `(Add | Set)-AdfsClient`

```PowerShell
Set-AdfsClient -LogoutUri <url>
```

Le `LogoutUri`est l’url utilisée par AF FS "session" l’utilisateur. Pour l’implémentation de la `LogoutUri`, le client doit s’assurer qu’il efface l’état d’authentification de l’utilisateur dans l’application, par exemple, la suppression de l’authentification jetons qu’il a. ADFS peuvent atteindre cette URL, avec le SID que le paramètre de requête, la partie de confiance de signalisation / application se déconnecter l’utilisateur. 

![](media/ad-fs-logout-openid-connect/adfs_single_logout2.png)


1.  **Jeton OAuth avec l’ID de session**: ADFS inclut l’id de session dans le jeton OAuth au moment de l’émission de jeton id_token. Cela servira ultérieurement par ADFS pour identifier les cookies SSO pertinentes pour être nettoyés pour l’utilisateur.
2.  **Utilisateur lance la déconnexion sur App1**: l’utilisateur peut initier une déconnexion de toutes les applications connectées. Dans cet exemple de scénario, un utilisateur lance une déconnexion d’App1.
3.  **Application envoie la demande de déconnexion à ADFS**: une fois que l’utilisateur lance la déconnexion, l’application envoie une requête GET à end_session_endpoint d’ADFS. L’application peut éventuellement inclure id_token_hint en tant que paramètre à cette demande. Si id_token_hint est présent, ADFS utilisera il conjointement avec l’ID de session à déconnecter le URI, le client doit être redirigé vers après la déconnexion (post_logout_redirect_uri).  Le post_logout_redirect_uri doit être un uri valide enregistré avec ADFS en utilisant le paramètre RedirectUris.
4.  **ADFS envoie déconnexion aux clients connectés**: utilise ADFS la valeur d’identificateur de session pour trouver les clients appropriés à l’utilisateur est connectée à. Les clients identifiés sont envoyés demande sur le LogoutUri enregistré avec ADFS pour lancer une déconnexion sur le côté client.

## <a name="faqs"></a>Forum aux questions
**Q:** je ne vois pas les paramètres frontchannel_logout_supported et frontchannel_logout_session_supported dans le document de découverte.</br>
**R:** Assurez-vous que vous avez [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801) installé sur tous les serveurs ADFS. Reportez-vous à la déconnexion unique dans Server2016 avec [KB4038801](https://support.microsoft.com/en-gb/help/4038801/windows-10-update-kb4038801).

**Q:** j’ai configuré déconnexion unique comme indiqué, mais utilisateur reste connecté complémentaire sur d’autres clients.</br>
**R:** vous assurer que `LogoutUri`est définie pour tous les clients où l’utilisateur est connecté. En outre, ADFS est une tentative d’envoyer la demande de déconnexion sur l’inscrit pire `LogoutUri`. Client doit implémenter la logique pour gérer la demande et prendre les mesures pour la déconnexion l’utilisateur à partir de l’application.</br>

**Q:** si après la déconnexion, un des clients remonte à ADFS avec un jeton d’actualisation valide, ADFS émet un jeton d’accès?</br>
**R:** Oui. Il incombe à l’application cliente de supprimer authentifiés tous les artefacts après une demande de déconnexion a été reçue à l’inscrit `LogoutUri`.


## <a name="next-steps"></a>Étapes suivantes
[Développement d’ADFS](../../ad-fs/AD-FS-Development.md)  
