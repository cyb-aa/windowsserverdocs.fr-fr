---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: AD FS les flux OpenID Connect/OAuth et les scénarios d’application
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d7ed8f7976116ab245fa730a5a050e7ec46cebea
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869491"
---
# <a name="ad-fs-openid-connectoauth-flows-and-application-scenarios"></a>AD FS les flux OpenID Connect/OAuth et les scénarios d’application
S’applique à AD FS 2016 et versions ultérieures


|Scénario|Procédure pas à pas de scénario utilisant des exemples|Canal/octroi OAuth 2,0|Type de client|
|-----|-----|-----|-----|
|Application à page unique</br> | &bull;[Exemple utilisant Adal](../development/Single-Page-Application-with-AD-FS.md)|[Implicit](#implicit-grant-flow)|Public| 
|Application Web qui connecte les utilisateurs</br> | &bull;[Exemple utilisant OWIN](../development/enabling-openid-connect-with-ad-fs.md)|[Code d’autorisation](#authorization-code-grant-flow)|Public, confidentiel|  
|L’application native appelle l’API Web</br>|&bull;[Exemple utilisant MSAL](../development/msal/adfs-msal-native-app-web-api.md)</br>&bull;[Exemple utilisant Adal](../development/native-client-with-ad-fs.md)|[Code d’autorisation](#authorization-code-grant-flow)|Public|   
|L’application Web appelle l’API Web</br>|&bull;[Exemple utilisant MSAL](../development/msal/adfs-msal-web-app-web-api.md)</br>&bull;[Exemple utilisant Adal](../development/enabling-oauth-confidential-clients-with-ad-fs.md)|[Code d’autorisation](#authorization-code-grant-flow)|Confidentiel| 
|L’API Web appelle une autre API Web pour le compte de (OBO) l’utilisateur</br>|&bull;[Exemple utilisant MSAL](../development/msal/adfs-msal-web-api-web-api.md)</br>&bull;[Exemple utilisant Adal](../development/ad-fs-on-behalf-of-authentication-in-windows-server.md)|[Pour le compte de](#on-behalf-of-flow)|L’application Web agit comme confidentielle| 
|L’application démon appelle l’API Web||[Informations d’identification du client](#client-credentials-grant-flow)|Confidentiel| 
|L’application Web appelle l’API Web à l’aide des CREDS utilisateur||[Informations d’identification du propriétaire de la ressource](#resource-owner-password-credentials-grant-flow-not-recommended)|Public, confidentiel| 
|L’application avec navigateur appelle l’API Web||[Code de l’appareil](#device-code-flow)|Public, confidentiel| 

## <a name="implicit-grant-flow"></a>Workflow d’octroi implicite 
 
Pour les applications à page unique (AngularJS, Ember. js, REACT. js, etc.), AD FS prend en charge le workflow d’octroi implicite OAuth 2,0. Le Flow implicite est décrit dans la [spécification OAuth 2,0](https://tools.ietf.org/html/rfc6749#section-4.2). Son principal avantage est qu’elle permet à l’application d’obtenir des jetons de AD FS sans effectuer d’échange d’informations d’identification de serveur principal. Cela permet à l’application de se connecter à l’utilisateur, de maintenir la session et de recevoir des jetons pour d’autres API Web dans le code JavaScript du client. Il existe quelques considérations importantes en matière de sécurité à prendre en compte lors de l’utilisation du Flow implicite spécifiquement autour du [client](https://tools.ietf.org/html/rfc6749#section-10.3).  
 
Si vous souhaitez utiliser le Flow et le AD FS implicites pour ajouter l’authentification à votre application JavaScript, suivez les étapes générales ci-dessous.  
  
### <a name="protocol-diagram"></a>Diagramme de protocole

Le diagramme suivant montre à quoi ressemble l’intégralité du déroulement de la connexion implicite et les sections qui suivent décrivent chaque étape plus en détail.  

![Connexion implicite](media/adfs-scenarios-for-developers/implicit.png)

### <a name="request-id-token-and-access-token"></a>Jeton d’ID de demande et jeton d’accès 
 
Pour connecter initialement l’utilisateur à votre application, vous pouvez envoyer une demande d’authentification OpenID Connect et obtenir id_token et un jeton d’accès à partir du point de terminaison AD FS.  
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=id_token+token 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&scope=openid 
&response_mode=fragment 
&state=12345 
```


|Paramètre|Obligatoire ou facultatif|Description| 
|-----|-----|-----|
|client_id|required|L’ID d’application (client) que le AD FS affecté à votre application.| 
|response_type|required|Doit inclure `id_token` pour la connexion à OpenID Connect. Il peut également inclure le response_type `token`. L’utilisation de Token ici permet à votre application de recevoir immédiatement un jeton d’accès à partir du point de terminaison Authorize sans avoir à effectuer une deuxième demande au point de terminaison de jeton.| 
|redirect_uri|required|La redirect_uri de votre application, où les réponses d’authentification peuvent être envoyées et reçues par votre application. Il doit correspondre exactement à l’un des redirection que vous avez configurés dans AD FS.| 
|nonce|required|Valeur incluse dans la demande, générée par l’application, qui sera incluse dans le id_token résultant en tant que revendication. L’application peut ensuite vérifier cette valeur pour atténuer les attaques par relecture de jetons. La valeur est généralement une chaîne unique et aléatoire qui peut être utilisée pour identifier l’origine de la demande. Obligatoire uniquement lorsqu’un id_token est demandé.|
|scope|facultatif|Liste d’étendues séparées par des espaces. Pour OpenID Connect, il doit inclure l’étendue `openid`.|
|resource|facultatif|URL de votre API Web.</br>Remarque : Si vous utilisez la bibliothèque cliente MSAL, le paramètre de ressource n’est pas envoyé. Au lieu de cela, l’URL de la ressource est envoyée dans le cadre du paramètre Scope :`scope = [resource url]//[scope values e.g., openid]`</br>Si la ressource n’est pas passée ici ou dans l’étendue, ADFS utilise une ressource par défaut urn : Microsoft : UserInfo. les stratégies de ressources UserInfo, telles que l’authentification MFA, l’émission ou la stratégie d’autorisation, ne peuvent pas être personnalisées.| 
|response_mode|facultatif| Spécifie la méthode à utiliser pour renvoyer le jeton résultant à votre application. La valeur par défaut est `fragment`.| 
|state|facultatif|Valeur incluse dans la demande qui est également retournée dans la réponse de jeton. Il peut s’agir d’une chaîne de tout contenu que vous souhaitez. Une valeur unique générée de manière aléatoire est généralement utilisée pour empêcher les attaques par falsification de requête intersites. L’État est également utilisé pour encoder les informations sur l’état de l’utilisateur dans l’application avant la demande d’authentification, comme la page ou la vue sur laquelle il se trouvait.| 
|prompt|facultatif|Indique le type d’interaction utilisateur requis. Les seules valeurs valides pour l’instant sont login et None.</br>- `prompt=login` forcera l’utilisateur à entrer ses informations d’identification sur cette demande, en annulant l’authentification unique. </br>- `prompt=none` est l’inverse. cela permet de s’assurer que l’utilisateur ne voit aucune invite interactive. Si la demande ne peut pas être effectuée en mode silencieux via l’authentification unique, AD FS renvoie une erreur interaction_required.| 
|login_hint|facultatif|Peut être utilisé pour préremplir le champ nom d’utilisateur/adresse de messagerie de la page de connexion de l’utilisateur, si vous connaissez son nom d’utilisateur à l’avance. Souvent, les applications utilisent ce paramètre lors de la réauthentification, après avoir extrait le nom d’utilisateur d’une connexion précédente `upn`à l' `id_token`aide de la revendication de.| 
|domain_hint|facultatif|Si elle est incluse, elle ignore le processus de découverte basé sur le domaine que l’utilisateur passe sur la page de connexion, ce qui se traduit par une expérience utilisateur légèrement plus rationalisée.| 

À ce stade, l’utilisateur est invité à entrer ses informations d’identification et à terminer l’authentification. Une fois que l’utilisateur s’est authentifié, le point de terminaison d’autorisation AD FS renvoie une réponse à votre application au niveau redirect_uri indiqué, à l’aide de la méthode spécifiée dans le paramètre response_mode.  
 
### <a name="successful-response"></a>Réponse réussie 
 
Une réponse correcte utilisant `response_mode=fragment and response_type=id_token+token` se présente comme suit :  
 
```
// Line breaks for legibility only 
 
GET https://localhost/myapp/# 
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZEstZnl0aEV... 
&token_type=Bearer 
&expires_in=3599 
&scope=openid  
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZstZnl0aEV1Q... 
&state=12345 
```


|Paramètre|Description| 
|-----|-----|
|access_token|Inclus si response_type inclut `token`.|
|token_type|Inclus si response_type inclut `token`. Sera toujours porteur.| 
|expires_in| Inclus si response_type inclut `token`. Indique le nombre de secondes pendant lesquelles le jeton est valide, à des fins de mise en cache.| 
|scope| Indique la ou les étendues pour lesquelles le access_token est valide.|  
|id_token|Inclus si response_type inclut `id_token`. Une JSON Web Token signée (JWT). L’application peut décoder les segments de ce jeton pour demander des informations sur l’utilisateur qui s’est connecté. L’application peut mettre en cache les valeurs et les afficher, mais elle ne doit pas s’en reposer sur les limites d’autorisation ou de sécurité.| 
|state|Si un paramètre d’État est inclus dans la demande, la même valeur doit apparaître dans la réponse. L’application doit vérifier que les valeurs d’état de la demande et de la réponse sont identiques.|

### <a name="refresh-tokens"></a>Jetons d’actualisation 
L’octroi implicite ne fournit pas de jetons d’actualisation.  `id_tokens` Et `access_tokens` expirent au bout d’un bref laps de temps. votre application doit donc être prête à actualiser ces jetons régulièrement. Pour actualiser l’un ou l’autre type de jeton, vous pouvez exécuter la même demande d' `prompt=none`iframe masquée ci-dessus à l’aide du paramètre pour contrôler le comportement de la plateforme d’identité. Si vous souhaitez recevoir un `new id_token`, veillez à utiliser. `response_type=id_token` 

## <a name="authorization-code-grant-flow"></a>Workflow d’octroi d’un code d’autorisation 
 
L’octroi du code d’autorisation OAuth 2,0 peut être utilisé dans les applications Web pour accéder aux ressources protégées, telles que les API Web. Le Flow code d’autorisation OAuth 2,0 est décrit dans [la section 4,1 de la spécification oauth 2,0](https://tools.ietf.org/html/rfc6749). Il est utilisé pour effectuer l’authentification et l’autorisation dans la majorité des types d’applications, notamment les applications Web et les applications installées en mode natif. Le Flow permet aux applications d’acquérir en toute sécurité des jetons qui peuvent être utilisés pour accéder aux ressources qui approuvent AD FS.  
 
### <a name="protocol-diagram"></a>Diagramme de protocole 
 
À un niveau élevé, le processus d’authentification pour une application native ressemble à ce qui suit :

![Workflow d’octroi d’un code d’autorisation](media/adfs-scenarios-for-developers/authorization.png)

### <a name="request-an-authorization-code"></a>Demander un code d’autorisation 
 
Le workflow de code d’autorisation commence par le client qui dirige l’utilisateur vers le point de terminaison/Authorize. Dans cette demande, le client indique les autorisations qu’il doit obtenir de la part de l’utilisateur : 
 
```
// Line breaks for legibility only 
 
https://adfs.contoso.com/adfs/oauth2/authorize? 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&response_type=code 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&response_mode=query 
&resource=https://webapi.com/ 
&scope=openid 
&state=12345 
```

|Paramètre|Obligatoire ou facultatif|Description|
|-----|-----|-----| 
|client_id|required|L’ID d’application (client) que le AD FS affecté à votre application.|  
|response_type|required| Doit inclure du code pour le workflow du code d’autorisation.| 
|redirect_uri|required|`redirect_uri` De votre application, où les réponses d’authentification peuvent être envoyées et reçues par votre application. Il doit correspondre exactement à l’un des redirection que vous avez enregistrés dans le AD FS pour le client.|  
|resource|facultatif|URL de votre API Web.</br>Remarque : Si vous utilisez la bibliothèque cliente MSAL, le paramètre de ressource n’est pas envoyé. Au lieu de cela, l’URL de la ressource est envoyée dans le cadre du paramètre Scope :`scope = [resource url]//[scope values e.g., openid]`</br>Si la ressource n’est pas passée ici ou dans l’étendue, ADFS utilise une ressource par défaut urn : Microsoft : UserInfo. les stratégies de ressources UserInfo, telles que l’authentification MFA, l’émission ou la stratégie d’autorisation, ne peuvent pas être personnalisées.| 
|scope|facultatif|Liste d’étendues séparées par des espaces.|
|response_mode|facultatif|Spécifie la méthode à utiliser pour renvoyer le jeton résultant à votre application. Il peut s'agir d'une des valeurs suivantes : </br>-requête </br>-fragment </br>- form_post</br>`query` fournit le code en tant que paramètre de chaîne de requête sur votre URI de redirection. Si vous demandez le code, vous pouvez utiliser Query, fragment ou form_post.  `form_post` exécuteunepublicationcontenantlecodedansvotre URI de redirection.|
|state|facultatif|Valeur incluse dans la demande qui est également retournée dans la réponse de jeton. Il peut s’agir d’une chaîne de tout contenu que vous souhaitez. Une valeur unique générée de manière aléatoire est généralement utilisée pour empêcher les attaques par falsification de requête intersites. La valeur peut également encoder les informations sur l’état de l’utilisateur dans l’application avant la demande d’authentification, comme la page ou la vue sur laquelle il se trouvait.|
|prompt|facultatif|Indique le type d’interaction utilisateur requis. Les seules valeurs valides pour l’instant sont login et None.</br>- `prompt=login` forcera l’utilisateur à entrer ses informations d’identification sur cette demande, en annulant l’authentification unique. </br>- `prompt=none` est l’inverse. cela permet de s’assurer que l’utilisateur ne voit aucune invite interactive. Si la demande ne peut pas être effectuée en mode silencieux via l’authentification unique, AD FS renvoie une erreur interaction_required.|
|login_hint|facultatif|Peut être utilisé pour préremplir le champ nom d’utilisateur/adresse de messagerie de la page de connexion de l’utilisateur, si vous connaissez son nom d’utilisateur à l’avance. Souvent, les applications utilisent ce paramètre lors de la réauthentification, après avoir extrait le nom d’utilisateur d’une connexion précédente `upn`à l' `id_token`aide de la revendication de.|
|domain_hint|facultatif|Si elle est incluse, elle ignore le processus de découverte basé sur le domaine que l’utilisateur passe sur la page de connexion, ce qui se traduit par une expérience utilisateur légèrement plus rationalisée.|
|code_challenge_method|facultatif|Méthode utilisée pour encoder le code_verifier pour le paramètre code_challenge. Peut avoir l'une des valeurs suivantes : </br>-plain </br>- S256 </br>S’il est exclu, code_challenge est supposé être en `code_challenge`texte brut si est inclus. AD FS prend en charge les types Plain et S256. Pour plus d’informations, consultez la [RFC PKCE](https://tools.ietf.org/html/rfc7636).|
|code_challenge|facultatif| Utilisé pour sécuriser les octrois de code d’autorisation via une clé de vérification pour l’échange de code (PKCE) à partir d’un client natif. Obligatoire si `code_challenge_method` est inclus. Pour plus d’informations, consultez la [RFC PKCE](https://tools.ietf.org/html/rfc7636)|

À ce stade, l’utilisateur est invité à entrer ses informations d’identification et à terminer l’authentification. Une fois que l’utilisateur s’est authentifié, le AD FS renvoie une réponse à votre application à `redirect_uri`l’adresse indiquée, à l’aide `response_mode`de la méthode spécifiée dans le paramètre.  
 
### <a name="successful-response"></a>Réponse réussie 
 
Une réponse correcte à l’aide de response_mode = Query ressemble à ceci : 
 
```
GET https://adfs.contoso.com/common/oauth2/nativeclient? 
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq... 
&state=12345  
```


|Paramètre|Description|
|-----|-----|
|code|`authorization_code` Que l’application a demandée. L’application peut utiliser le code d’autorisation pour demander un jeton d’accès pour la ressource cible. Les les codes présentent sont de courte durée, généralement elles expirent au bout de 10 minutes environ.|
|state|Si un `state` paramètre est inclus dans la demande, la même valeur doit apparaître dans la réponse. L’application doit vérifier que les valeurs d’état de la demande et de la réponse sont identiques.|

### <a name="request-an-access-token"></a>Demander un jeton d’accès 
 
Maintenant que vous avez acquis un `authorization_code` et que vous avez reçu une autorisation de la part de l’utilisateur, vous pouvez `access_token`échanger le code d’un à la ressource souhaitée. Pour ce faire, envoyez une demande de publication au point de terminaison/Token :  
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com/ 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr... 
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 
&grant_type=authorization_code 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for confidential clients (web apps)  
```

|Paramètre|Obligatoire/facultatif|Description|
|-----|-----|-----| 
|client_id|required|L’ID d’application (client) que le AD FS affecté à votre application.| 
|grant_type|required|Doit être `authorization_code` pour le workflow du code d’autorisation.| 
|code|required|`authorization_code` Que vous avez acquis dans le premier tronçon du Flow.| 
|redirect_uri|required|La valeur qui a été utilisée pour acquérir le `authorization_code`. `redirect_uri`| 
|client_secret|requis pour les applications Web|Le secret d’application que vous avez créé lors de l’inscription de l’application dans AD FS. Vous ne devez pas utiliser le secret d’application dans une application native, car clés secrètes client ne peut pas être stocké de manière fiable sur des appareils. Elle est requise pour les applications Web et les API Web, qui ont la capacité de stocker les client_secret de façon sécurisée côté serveur. La clé secrète client doit être encodée URL avant d’être envoyée. Ces applications peuvent également utiliser une authentification basée sur les clés en signant un JWT et en l’ajoutant en tant que paramètre client_assertion.| 
|code_verifier|facultatif|Identique `code_verifier` à celui utilisé pour obtenir le autorisation. Obligatoire si PKCE a été utilisé dans la demande d’octroi de code d’autorisation. Pour plus d’informations, consultez la [RFC PKCE](https://tools.ietf.org/html/rfc7636).</br>Remarque : s’applique à AD FS 2019 et versions ultérieures| 

### <a name="successful-response"></a>Réponse réussie 
 
Une réponse de jeton réussie se présente comme suit : 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
} 
```


|Paramètre|Description| 
|-----|-----|
|access_token|Le jeton d’accès demandé. L’application peut utiliser ce jeton pour s’authentifier auprès de la ressource sécurisée (API Web).| 
|token_type|Indique la valeur du type de jeton. Le seul type pris en charge par AD FS est Bearer.
|expires_in|Durée de validité du jeton d’accès (en secondes).
|refresh_token|Jeton d’actualisation OAuth 2,0. L’application peut utiliser ce jeton pour acquérir des jetons d’accès supplémentaires après l’expiration du jeton d’accès actuel. Les jetons sont de longue durée et peuvent être utilisés pour conserver l’accès aux ressources pendant des périodes prolongées.| 
|refresh_token_expires_in|Durée de validité du jeton d’actualisation (en secondes).| 
|id_token|JSON Web Token (JWT). L’application peut décoder les segments de ce jeton pour demander des informations sur l’utilisateur qui s’est connecté. L’application peut mettre en cache les valeurs et les afficher, mais elle ne doit pas s’en servir pour les limites d’autorisation ou de sécurité.|

### <a name="use-the-access-token"></a>Utiliser le jeton d’accès 
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q... 
 ```

### <a name="refresh-the-access-token"></a>Actualiser le jeton d’accès 
 
Les jetons sont à courte durée de vie et vous devez les actualiser après leur expiration pour continuer à accéder aux ressources. Pour ce faire, vous devez envoyer une autre demande de `/token`publication au point de terminaison, en fournissant la valeur refresh_token au lieu du code. Les jetons d’actualisation sont valides pour toutes les autorisations pour lesquelles votre client a déjà reçu un jeton d’accès. 
 
Les jetons d’actualisation n’ont pas de durée de vie spécifiée. En règle générale, les durées de vie des jetons d’actualisation sont relativement longues. Toutefois, dans certains cas, les jetons d’actualisation expirent, sont révoqués ou ne disposent pas de privilèges suffisants pour l’action souhaitée. Votre application doit attendre et gérer correctement les erreurs retournées par le point de terminaison d’émission de jeton.  
 
Bien que les jetons d’actualisation ne soient pas révoqués lorsqu’ils sont utilisés pour acquérir de nouveaux jetons d’accès, vous devez ignorer l’ancien jeton d’actualisation. La spécification OAuth 2,0 indique : «Le serveur d’autorisation peut émettre un nouveau jeton d’actualisation, auquel cas le client doit ignorer l’ancien jeton d’actualisation et le remplacer par le nouveau jeton d’actualisation. Le serveur d’autorisation peut révoquer l’ancien jeton d’actualisation après l’émission d’un nouveau jeton d’actualisation au client.» 
 
```
// Line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq... 
&grant_type=refresh_token 
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for confidential clients (web apps)  
```


|Paramètre|Obligatoire ou facultatif|Description| 
|-----|-----|-----|
|client_id|required|L’ID d’application (client) que le AD FS affecté à votre application.| 
|grant_type|required|Doit être `refresh_token` pour cette branche du workflow de code d’autorisation.| 
|resource|facultatif|URL de votre API Web.</br>Remarque : Si vous utilisez la bibliothèque cliente MSAL, le paramètre de ressource n’est pas envoyé. Au lieu de cela, l’URL de la ressource est envoyée dans le cadre du paramètre Scope :`scope = [resource url]//[scope values e.g., openid]`</br>Si la ressource n’est pas passée ici ou dans l’étendue, ADFS utilise une ressource par défaut urn : Microsoft : UserInfo. les stratégies de ressources UserInfo, telles que l’authentification MFA, l’émission ou la stratégie d’autorisation, ne peuvent pas être personnalisées.|
|scope|facultatif|Liste d’étendues séparées par des espaces.| 
|refresh_token|required|Refresh_token que vous avez acquis au cours de la deuxième jambe du Flow.| 
|client_secret|requis pour les applications Web| Le secret d’application que vous avez créé dans le portail d’inscription des applications pour votre application. Il ne doit pas être utilisé dans une application native, car clés secrètes client ne peut pas être stocké de manière fiable sur des appareils. Elle est requise pour les applications Web et les API Web, qui ont la capacité de stocker les client_secret de façon sécurisée côté serveur. Ces applications peuvent également utiliser une authentification basée sur les clés en signant un JWT et en l’ajoutant en tant que paramètre client_assertion.|

### <a name="successful-response"></a>Réponse réussie 
Une réponse de jeton réussie se présente comme suit : 
 
```
{ 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...", 
    "token_type": "Bearer", 
    "expires_in": 3599, 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...", 
}  
```
|Paramètre|Description| 
|-----|-----|
|access_token|Le jeton d’accès demandé. L’application peut utiliser ce jeton pour s’authentifier auprès de la ressource sécurisée, telle qu’une API Web.| 
|token_type|Indique la valeur du type de jeton. Le seul type pris en charge par AD FS est Bearer|
|expires_in|Durée de validité du jeton d’accès (en secondes).|
|scope|Étendues pour lesquelles le access_token est valide.| 
|refresh_token|Jeton d’actualisation OAuth 2,0. L’application peut utiliser ce jeton pour acquérir des jetons d’accès supplémentaires après l’expiration du jeton d’accès actuel. Les jetons sont de longue durée et peuvent être utilisés pour conserver l’accès aux ressources pendant des périodes prolongées.| 
|refresh_token_expires_in|Durée de validité du jeton d’actualisation (en secondes).| 
|id_token|JSON Web Token (JWT). L’application peut décoder les segments de ce jeton pour demander des informations sur l’utilisateur qui s’est connecté. L’application peut mettre en cache les valeurs et les afficher, mais elle ne doit pas s’en servir pour les limites d’autorisation ou de sécurité.|

## <a name="on-behalf-of-flow"></a>Flow pour le compte de 
 
Le OBO OAuth 2,0 est utilisé dans le cas d’utilisation où une application appelle un service ou une API Web, qui à son tour doit appeler un autre service/API Web. L’idée est de propager l’identité de l’utilisateur délégué et les autorisations via la chaîne de requête. Pour que le service de niveau intermédiaire effectue des demandes authentifiées auprès du service en aval, il doit sécuriser un jeton d’accès à partir de la AD FS, pour le compte de l’utilisateur.  
 
### <a name="protocol-diagram"></a>Diagramme de protocole 
Supposons que l’utilisateur a été authentifié sur une application à l’aide du processus d’octroi de code d’autorisation OAuth 2,0 décrit ci-dessus. À ce stade, l’application dispose d’un jeton d’accès pour l’API A (jeton A) avec les revendications et le consentement de l’utilisateur pour accéder à l’API Web de niveau intermédiaire (API A). Assurez-vous que le client demande l’étendue user_impersonation dans le jeton. Désormais, l’API A doit effectuer une demande authentifiée à l’API Web en aval (API B). 

Les étapes qui suivent constituent le OBO Flow et sont expliquées dans l’aide du diagramme suivant. 

![Flow pour le compte de](media/adfs-scenarios-for-developers/obo.png)

  1. L’application cliente envoie une requête à l’API A avec le jeton A.  
  Remarque : Lors de la configuration du OBO Flow dans AD FS Assurez-vous que l’option `user_impersonation` étendue `user_impersonation` est sélectionnée et que le client demande l’étendue dans la demande. 
  2. L’API A s’authentifie auprès du point de terminaison d’émission de jetons AD FS et demande un jeton pour accéder à l’API B. Remarque : Lors de la configuration de ce Workflow dans AD FS Assurez-vous que l’API A est également inscrite en tant qu’application serveur avec clientID ayant la même valeur que l’ID de ressource dans l’API A. Pour plus d’informations, reportez-vous à pour le compte de l’exemple ici ajouter un lien.  
  3. Le point de terminaison d’émission de jetons AD FS valide les informations d’identification de l’API A avec le jeton A et émet le jeton d’accès pour l’API B (jeton B). 
  4. Le jeton B est défini dans l’en-tête d’autorisation de la demande à l’API B. 
  5. Les données de la ressource sécurisée sont retournées par l’API B. 

### <a name="service-to-service-access-token-request"></a>Demande de jeton d’accès de service à service 
 
Pour demander un jeton d’accès, effectuez une publication HTTP sur le point de terminaison de jeton AD FS avec les paramètres suivants.  


### <a name="first-case-access-token-request-with-a-shared-secret"></a>Premier cas: Demande de jeton d’accès avec un secret partagé 
 
Lors de l’utilisation d’un secret partagé, une demande de jeton d’accès de service à service contient les paramètres suivants: 


|Paramètre|Obligatoire ou facultatif|Description|
|-----|-----|-----| 
|grant_type|required|Type de demande de jeton. Pour une demande utilisant un JWT, la valeur doit être urn : IETF : params : OAuth : Grant-type : JWT-Bearer.|  
|client_id|required|L’ID client que vous configurez lors de l’inscription de votre première API Web en tant qu’application serveur (application de niveau intermédiaire). Il doit s’agir du même ID de ressource que celui utilisé dans la première jambe, c.-à-d. URL de la première API Web.| 
|client_secret|required|Le secret d’application que vous avez créé lors de l’inscription de l’application serveur dans AD FS.| 
|assertion|required|Valeur du jeton utilisé dans la demande.|  
|requested_token_use|required|Spécifie le mode de traitement de la demande. Dans le OBO Flow, la valeur doit être définie sur on_behalf_of| 
|resource|required|L’ID de ressource fourni lors de l’inscription de la première API Web en tant qu’application serveur (application de niveau intermédiaire). L’ID de ressource doit être l’URL de la deuxième application de niveau intermédiaire de l’API Web qui appellera pour le compte du client.|
|scope|facultatif|Liste d’étendues séparées par des espaces pour la demande de jeton.| 

#### <a name="example"></a>Exemple 
 
L’exemple `HTTP POST` suivant demande un jeton d’accès et un jeton d’actualisation 
 
```
//line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer 
&client_id=https://webapi.com/ 
&client_secret=BYyVnAt56JpLwUcyo47XODd 
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIm… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope=openid    
```

### <a name="second-case-access-token-request-with-a-certificate"></a>Deuxième cas: Demande de jeton d’accès avec un certificat 
 
Une demande de jeton d’accès de service à service avec un certificat contient les paramètres suivants: 


|Paramètre|obligatoire/facultatif|Description|
|-----|-----|-----| 
|grant_type|required|Type de demande de jeton. Pour une demande utilisant un JWT, la valeur doit être urn : IETF : params : OAuth : Grant-type : JWT-Bearer. |
|client_id|required|L’ID client que vous configurez lors de l’inscription de votre première API Web en tant qu’application serveur (application de niveau intermédiaire). Il doit s’agir du même ID de ressource que celui utilisé dans la première jambe, c.-à-d. URL de la première API Web.|  
|client_assertion_type|required|La valeur doit être urn : IETF : params : OAuth : client-assertion-type : JWT-Bearer.| 
|client_assertion|required|Une assertion (un jeton Web JSON) que vous devez créer et signer avec le certificat que vous avez enregistré en tant qu’informations d’identification pour votre application.|  
|assertion|required|Valeur du jeton utilisé dans la demande.| 
|requested_token_use|required|Spécifie le mode de traitement de la demande. Dans le OBO Flow, la valeur doit être définie sur on_behalf_of| 
|resource|required|L’ID de ressource fourni lors de l’inscription de la première API Web en tant qu’application serveur (application de niveau intermédiaire). L’ID de ressource doit être l’URL de la deuxième application de niveau intermédiaire de l’API Web qui appellera pour le compte du client.|
|scope|facultatif|Liste d’étendues séparées par des espaces pour la demande de jeton.|


Notez que les paramètres sont presque les mêmes que dans le cas de la demande par secret partagé, sauf que le paramètre client_secret est remplacé par deux paramètres : client_assertion_type et client_assertion. 

#### <a name="example"></a>Exemple 
La requête HTTP suivante demande un jeton d’accès pour l’API Web avec un certificat.

``` 
// line breaks for legibility only 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer 
&client_id= https://webapi.com/ 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNS… 
&resource=https://secondwebapi.com/
&requested_token_use=on_behalf_of
&scope= openid 
```    

### <a name="service-to-service-access-token-response"></a>Réponse de jeton d’accès de service à service 
 
Une réponse de réussite est une réponse JSON OAuth 2,0 avec les paramètres suivants. 


|Paramètre|Description|
|-----|-----| 
|token_type|Indique la valeur du type de jeton. Le seul type pris en charge par AD FS est Bearer. | 
|scope|Étendue de l’accès accordé dans le jeton.| 
|expires_in|Durée, en secondes, pendant laquelle le jeton d’accès est valide.| 
|access_token|Le jeton d’accès demandé. Le service appelant peut utiliser ce jeton pour s’authentifier auprès du service de réception.| 
|id_token|JSON Web Token (JWT). L’application peut décoder les segments de ce jeton pour demander des informations sur l’utilisateur qui s’est connecté. L’application peut mettre en cache les valeurs et les afficher, mais elle ne doit pas s’en servir pour les limites d’autorisation ou de sécurité.| 
|refresh_token|Jeton d’actualisation pour le jeton d’accès demandé. Le service appelant peut utiliser ce jeton pour demander un autre jeton d’accès après l’expiration du jeton d’accès actuel.|
|Refresh_token_expires_in|Durée, en secondes, pendant laquelle le jeton d’actualisation est valide. 

### <a name="success-response-example"></a>Exemple de réponse de réussite 
 
L’exemple suivant montre une réponse de réussite à une demande de jeton d’accès pour l’API Web. 

``` 
{ 
  "token_type": "Bearer", 
  "scope": openid, 
  "expires_in": 3269, 
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1t" 
  "id_token": "aWRfdG9rZW49ZXlKMGVYQWlPaUpLVjFRaUxDSmhiR2NpT2lKU1V6STFOa" 
  "refresh_token": "OAQABAAAAAABnfiG…" 
  "refresh_token_expires_in": 28800, 
} 
```  
 
 
Utilisez le jeton d’accès pour accéder à la ressource sécurisée maintenant le service de niveau intermédiaire peut utiliser le jeton acquis ci-dessus pour effectuer des demandes authentifiées à l’API Web en aval, en définissant le jeton dans l’en-tête d’autorisation.  

#### <a name="example"></a>Exemple 
``` 
GET /v1.0/me HTTP/1.1 
Host: https://secondwebapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQ… 
``` 

## <a name="client-credentials-grant-flow"></a>Workflow d’octroi des informations d’identification du client 
 
Vous pouvez utiliser l’octroi d’informations d’identification du client OAuth 2,0 spécifié dans la [RFC 6749](https://tools.ietf.org/html/rfc6749#section-4.4), pour accéder aux ressources hébergées sur le Web à l’aide de l’identité d’une application. Ce type d’octroi est couramment utilisé pour les interactions de serveur à serveur qui doivent s’exécuter en arrière-plan, sans interaction immédiate avec un utilisateur. Ces types d’applications sont souvent appelés démons ou comptes de service. 

Le processus d’octroi d’informations d’identification du client OAuth 2,0 permet à un service Web (client confidentiel) d’utiliser ses propres informations d’identification, au lieu d’emprunter l’identité d’un utilisateur, pour s’authentifier lors de l’appel d’un autre service Web. Dans ce scénario, le client est généralement un service Web de niveau intermédiaire, un service démon ou un site Web. Pour un niveau d’assurance plus élevé, le AD FS permet également au service appelant d’utiliser un certificat (au lieu d’un secret partagé) comme informations d’identification. 

### <a name="protocol-diagram"></a>Diagramme de protocole 

Le diagramme suivant illustre le processus d’octroi des informations d’identification du client. 

![Informations d’identification du client](media/adfs-scenarios-for-developers/credentials.png)

### <a name="request-a-token"></a>Demander un jeton 
 
Pour obtenir un jeton à l’aide de l’octroi des informations d’identification `POST` du client, envoyez une demande au point de terminaison/Token AD FS :  
 
### <a name="first-case-access-token-request-with-a-shared-secret"></a>Premier cas: Demande de jeton d’accès avec un secret partagé 
 
```
POST /adfs/oauth2/token HTTP/1.1            
//Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
client_id=535fb089-9ff3-47b6-9bfb-4f1264799865 
&client_secret=qWgdYAmab0YSkuL1qKv5bPX 
&grant_type=client_credentials 
```

|Paramètre|Obligatoire ou facultatif|Description|
|-----|-----|-----| 
|client_id|required|L’ID d’application (client) que le AD FS affecté à votre application.| 
|scope|facultatif|Liste séparée par des espaces des étendues auxquelles l’utilisateur doit donner son consentement.| 
|client_secret|required|La clé secrète client que vous avez générée pour votre application dans le portail d’inscription des applications. La clé secrète client doit être encodée URL avant d’être envoyée.| 
|grant_type|required|Doit avoir la valeur `client_credentials`.|

### <a name="second-case-access-token-request-with-a-certificate"></a>Deuxième cas: Demande de jeton d’accès avec un certificat 

``` 
POST /adfs/oauth2/token HTTP/1.1                
 
// Line breaks for clarity 
 
Host: https://adfs.contoso.com 
Content-Type: application/x-www-form-urlencoded 
 
&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05 
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer 
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg 
&grant_type=client_credentials  
```

|Paramètre|Obligatoire ou facultatif|Description| 
|-----|-----|-----|
|client_assertion_type|required|La valeur doit être définie sur urn : IETF : params : OAuth : client-assertion-type : JWT-Bearer.| 
|client_assertion|required|Une assertion (un jeton Web JSON) que vous devez créer et signer avec le certificat que vous avez enregistré en tant qu’informations d’identification pour votre application.|  
|grant_type|required|Doit avoir la valeur `client_credentials`.|
|client_id|facultatif|L’ID d’application (client) que le AD FS affecté à votre application. Cela fait partie de client_assertion. il n’est donc pas nécessaire de la transmettre ici.| 
|scope|facultatif|Liste séparée par des espaces des étendues auxquelles l’utilisateur doit donner son consentement.| 

### <a name="use-a-token"></a>Utiliser un jeton 
 
Maintenant que vous avez acquis un jeton, utilisez le jeton pour effectuer des demandes à la ressource. Lorsque le jeton expire, répétez la requête sur le point de terminaison/Token pour obtenir un nouveau jeton d’accès.  
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...  
```

## <a name="resource-owner-password-credentials-grant-flow-not-recommended"></a>Workflow d’octroi des informations d’identification du mot de passe du propriétaire de la ressource (non recommandé) 
 
L’octroi d’informations d’identification de mot de passe de propriétaire de ressource (ROPC) permet à une application de se connecter à l’utilisateur en gérant directement son mot de passe. Le flux ROPC nécessite un degré élevé de confiance et d’exposition de l’utilisateur. vous ne devez utiliser ce flux que lorsque d’autres flux, plus sécurisés, ne peuvent pas être utilisés.  
 
### <a name="protocol-diagram"></a>Diagramme de protocole 
 
Le diagramme suivant illustre le déroulement du ROPC.

![ROPC Flow](media/adfs-scenarios-for-developers/resource.png)

### <a name="authorization-request"></a>Demande d’autorisation 
Le ROPC est une requête unique : il envoie l’identification du client et les informations d’identification de l’utilisateur au IDP, puis reçoit les jetons en retour. Le client doit demander l’adresse de messagerie de l’utilisateur (UPN) et le mot de passe avant de procéder. Immédiatement après une demande réussie, le client doit libérer en toute sécurité les informations d’identification de l’utilisateur à partir de la mémoire. Il ne doit jamais les enregistrer.  

```
// Line breaks and spaces are for legibility only. 
 
POST /adfs/oauth2/token HTTP/1.1 
Host: https://adfs.contoso.com  
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
&scope= openid  
&username=myusername@contoso.com 
&password=SuperS3cret 
&grant_type=password 
```


|Paramètre|Obligatoire ou facultatif|Description| 
|-----|-----|-----|
|client_id|required|ID de client| 
|grant_type|required|Doit être défini sur mot de passe.| 
|userName|required|Adresse de messagerie de l’utilisateur.| 
|password|required|Mot de passe de l’utilisateur.| 
|scope|facultatif|Liste d’étendues séparées par des espaces.|

### <a name="successful-authentication-response"></a>Réponse d’authentification réussie 
L’exemple suivant montre une réponse de jeton réussie : 

```
{ 
    "token_type": "Bearer", 
    "scope": "openid", 
    "expires_in": 3599, 
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIn...", 
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...", 
    "refresh_token_expires_in": 28800, 
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDR..." 
}  
```


|Paramètre|Description| 
|-----|-----|
|token_type|Toujours défini sur Bearer.| 
|scope|Si un jeton d’accès a été retourné, ce paramètre répertorie les étendues pour lesquelles le jeton d’accès est valide.| 
|expires_in|Nombre de secondes pendant lesquelles le jeton d’accès inclus est valide.| 
|access_token|Émis pour les étendues qui ont été demandées.| 
|id_token|JSON Web Token (JWT). L’application peut décoder les segments de ce jeton pour demander des informations sur l’utilisateur qui s’est connecté. L’application peut mettre en cache les valeurs et les afficher, mais elle ne doit pas s’en servir pour les limites d’autorisation ou de sécurité.| 
|refresh_token_expires_in|Nombre de secondes pendant lesquelles le jeton d’actualisation inclus est valide.| 
|refresh_token|Émis si le paramètre d’étendue d’origine incluait offline_access.|

Vous pouvez utiliser le jeton d’actualisation pour acquérir de nouveaux jetons d’accès et des jetons d’actualisation en utilisant le même processus que celui décrit dans la section relative au fluide d’octroi de code d’authentification ci-dessus.   

## <a name="device-code-flow"></a>Workflow de code de l’appareil 
 
L’octroi de code d’appareil permet aux utilisateurs de se connecter à des appareils avec restriction d’entrée tels qu’une télévision intelligente, un appareil IoT ou une imprimante. Pour activer ce processus, l’utilisateur visite une page Web dans son navigateur sur un autre appareil pour se connecter. Une fois que l’utilisateur se connecte, l’appareil peut recevoir des jetons d’accès et des jetons d’actualisation en fonction des besoins. 
 
### <a name="protocol-diagram"></a>Diagramme de protocole 
 
L’intégralité du workflow de code de l’appareil ressemble au diagramme suivant. Nous décrivons chacune des étapes plus loin dans cet article. 
 
![Workflow de code de l’appareil](media/adfs-scenarios-for-developers/device.png)

### <a name="device-authorization-request"></a>Demande d’autorisation de l’appareil 
Le client doit d’abord vérifier auprès du serveur d’authentification s’il s’agit d’un périphérique et d’un code utilisateur utilisés pour initier l’authentification. Le client collecte cette demande à partir du point de terminaison/devicecode. Dans cette demande, le client doit également inclure les autorisations qu’il doit obtenir de la part de l’utilisateur. À partir du moment où cette demande est envoyée, l’utilisateur ne peut se connecter qu’à 15 minutes (la valeur habituelle pour expires_in). par conséquent, n’effectuez cette demande que lorsque l’utilisateur a indiqué qu’elle est prête à se connecter. 

```
// Line breaks are for legibility only. 
 
POST https://adfs.contoso.com/adfs/oauth2/devicecode 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
scope=openid 
```


|Paramètre|Condition|Description|
|-----|-----|-----| 
|client_id|required|L’ID d’application (client) que le AD FS affecté à votre application.| 
|scope|facultatif|Liste d’étendues séparées par des espaces.|

### <a name="device-authorization-response"></a>Réponse d’autorisation de l’appareil 
Une réponse correcte est un objet JSON contenant les informations requises pour permettre à l’utilisateur de se connecter. 


|Paramètre|Description|
|-----|-----| 
|device_code|Chaîne longue utilisée pour vérifier la session entre le client et le serveur d’autorisation. Le client utilise ce paramètre pour demander le jeton d’accès du serveur d’autorisation.| 
|user_code|Chaîne brève affichée à l’utilisateur qui est utilisée pour identifier la session sur un appareil secondaire.| 
|verification_uri|URI auquel l’utilisateur doit accéder avec user_code pour se connecter.| 
|verification_uri_complete|URI auquel l’utilisateur doit accéder avec user_code pour se connecter. Il est prérempli avec user_code pour que l’utilisateur n’ait pas besoin d’entrer user_code| 
|expires_in|Nombre de secondes avant l’expiration de device_code et user_code.| 
|interval|Nombre de secondes pendant lesquelles le client doit attendre entre les demandes d’interrogation.| 
|message|Chaîne explicite avec des instructions pour l’utilisateur. Cela peut être localisé en incluant un paramètre de requête dans la demande du formulaire ? MKT = XX-XX, en remplissant le code de culture de langue approprié.  

### <a name="authenticating-the-user"></a>Authentification de l’utilisateur 
Après avoir reçu les user_code et verification_uri, le client les affiche à l’utilisateur, ce qui leur demande de se connecter à l’aide de leur téléphone mobile ou navigateur de PC. En outre, le client peut utiliser un code QR ou un mécanisme similaire pour afficher verfication_uri_complete, ce qui va à l’étape de la saisie des user_code pour l’utilisateur. Pendant que l’utilisateur s’authentifie sur le verification_uri, le client doit interroger le point de terminaison/Token pour le jeton demandé à l’aide de device_code. 

```
POST https://adfs.contoso.com /adfs/oauth2/token 
Content-Type: application/x-www-form-urlencoded 
 
grant_type: urn:ietf:params:oauth:grant-type:device_code 
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e 
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8 
```


|Paramètre|required|Description|
|-----|-----|-----| 
|grant_type|required|Doit être urn : IETF : params : OAuth : Grant-type : device_code| 
|client_id|required|Doit correspondre au client_id utilisé dans la demande initiale.| 
|code|required|Device_code retourné dans la demande d’autorisation de l’appareil.|

### <a name="successful-authentication-response"></a>Réponse d’authentification réussie 
Une réponse de jeton réussie se présente comme suit :  


|Paramètre|Description|
|-----|-----| 
|token_type|Toujours «porteur.| 
|scope|Si un jeton d’accès a été retourné, cette liste répertorie les étendues pour lesquelles le jeton d’accès est valide.| 
|expires_in|Nombre de secondes avant que le jeton d’accès inclus ne soit valide pour.| 
|access_token|Émis pour les étendues qui ont été demandées.| 
|id_token|Émis si le paramètre d’étendue d’origine incluait l’étendue OpenID.| 
|refresh_token|Émis si le paramètre d’étendue d’origine incluait offline_access.| 
|refresh_token_expires_in|Nombre de secondes avant que le jeton d’actualisation inclus ne soit valide pour.| 


## <a name="related-content"></a>Contenu connexe  
Pour obtenir la liste complète des Articles de procédure pas à pas, consultez [AD FS développement](../AD-FS-Development.md) , qui fournit des instructions pas à pas sur l’utilisation des flux associés. 
