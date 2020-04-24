---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: Flux OpenID Connect/OAuth avec AD FS et scénarios d’application
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0b7fef9c74ba5da1b94772cb5f6ff3d717a5359
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855972"
---
# <a name="ad-fs-openid-connectoauth-flows-and-application-scenarios"></a>Flux OpenID Connect/OAuth avec AD FS et scénarios d’application
S’applique à AD FS 2016 et versions ultérieures


|Scénario|Procédure pas à pas de scénario utilisant des exemples|Canal/octroi OAuth 2.0|Type de client|
|-----|-----|-----|-----|
|Application monopage</br> | &bull; [Exemple utilisant ADAL](../development/Single-Page-Application-with-AD-FS.md)|[Implicite](#implicit-grant-flow)|Public| 
|Application web qui connecte des utilisateurs</br> | &bull; [Exemple utilisant OWIN](../development/enabling-openid-connect-with-ad-fs.md)|[Code d’autorisation](#authorization-code-grant-flow)|Public, confidentiel|  
|Une application native appelle une API web</br>|&bull; [Exemple utilisant MSAL](../development/msal/adfs-msal-native-app-web-api.md)</br>&bull; [Exemple utilisant ADAL](../development/native-client-with-ad-fs.md)|[Code d’autorisation](#authorization-code-grant-flow)|Public|   
|Une application web appelle une API web</br>|&bull; [Exemple utilisant MSAL](../development/msal/adfs-msal-web-app-web-api.md)</br>&bull; [Exemple utilisant ADAL](../development/enabling-oauth-confidential-clients-with-ad-fs.md)|[Code d’autorisation](#authorization-code-grant-flow)|Confidentiel| 
|Une API web appelle une autre API web pour le compte de l’utilisateur</br>|&bull; [Exemple utilisant MSAL](../development/msal/adfs-msal-web-api-web-api.md)</br>&bull; [Exemple utilisant ADAL](../development/ad-fs-on-behalf-of-authentication-in-windows-server.md)|[On-behalf-of](#on-behalf-of-flow)|L’application web agit en tant que confidentielle| 
|L’application démon appelle une API web||[Informations d’identification du client](#client-credentials-grant-flow)|Confidentiel| 
|Une application web appelle une API web à l’aide d’informations d’identification utilisateur||[Informations d’identification du mot de passe du propriétaire de la ressource](#resource-owner-password-credentials-grant-flow-not-recommended)|Public, confidentiel| 
|Une application sans navigateur appelle une API web||[Code d’appareil](#device-code-flow)|Public, confidentiel| 

## <a name="implicit-grant-flow"></a>Octroi de flux implicite 
 
Pour les applications monopages (AngularJS, Ember.js, React.js, et ainsi de suite), AD FS prend en charge le flux d’octroi implicite OAuth 2.0. Le flux implicite est décrit dans la  [Spécification OAuth 2.0](https://tools.ietf.org/html/rfc6749#section-4.2). Son principal avantage est qu’il permet à l’application d’obtenir des jetons à partir d’AD FS sans effectuer d’échange d’informations d’identification de serveur back-end. Cela permet à l’application de connecter l’utilisateur, de maintenir la session et de recevoir des jetons pour d’autres API web dans le code JavaScript du client. Il existe quelques considérations importantes en matière de sécurité à prendre en compte lors de l’utilisation du flux implicite, spécifiquement liées au  [client](https://tools.ietf.org/html/rfc6749#section-10.3).  
 
Si vous souhaitez utiliser le flux implicite et AD FS pour ajouter l’authentification à votre application JavaScript, effectuez les étapes générales ci-dessous.  
  
### <a name="protocol-diagram"></a>Diagramme de protocole

Le diagramme suivant illustre l’intégralité du flux de connexion implicite, et les sections qui suivent décrivent chaque étape plus en détail.  

![Connexion implicite](media/adfs-scenarios-for-developers/implicit.png)

### <a name="request-id-token-and-access-token"></a>Demander le jeton d’ID et le jeton d’accès 
 
Pour connecter initialement l’utilisateur à votre application, vous pouvez envoyer une requête d’authentification OpenID Connect et obtenir id_token et un jeton d’accès à partir du point de terminaison AD FS.  
 
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
|client_id|obligatoire|ID d’application (client) qu’AD FS a affecté à votre application.| 
|response_type|obligatoire|Doit inclure  `id_token` pour la connexion à OpenID Connect. Peut également inclure response_type  `token`. L’utilisation de token ici permet à votre application de recevoir immédiatement un jeton d’accès à partir du point de terminaison d’autorisation, sans avoir à effectuer de deuxième requête au point de terminaison de jeton.| 
|redirect_uri|obligatoire|redirect_uri de votre application, où les réponses d’authentification peuvent être envoyées et reçues par votre application. Doit correspondre exactement à l’un des redirect_uris que vous avez configurés dans AD FS.| 
|nonce|obligatoire|Valeur incluse dans la requête, générée par l’application, qui sera incluse dans l’id_token résultant en tant que revendication. L’application peut ensuite vérifier cette valeur pour atténuer les attaques par relecture de jetons. La valeur est généralement une chaîne unique et aléatoire qui peut être utilisée pour identifier l’origine de la requête. Obligatoire uniquement quand un id_token est demandé.|
|scope|facultatif|Liste d’étendues séparées par des espaces. Pour OpenID Connect, elle doit inclure l’étendue  `openid`.|
|resource|facultatif|URL de votre API web.</br>Remarque : Si vous utilisez la bibliothèque de client MSAL, le paramètre de ressource n’est pas envoyé. Au lieu de cela, l’URL de la ressource est envoyée dans le cadre du paramètre scope : `scope = [resource url]//[scope values e.g., openid]`</br>Si la ressource n’est pas passée ici ou dans l’étendue, ADFS utilise une ressource par défaut urn:microsoft:userinfo. Les stratégies de ressources userinfo, telles que MFA, Émission ou stratégie d’autorisation, ne peuvent pas être personnalisées.| 
|response_mode|facultatif| Spécifie la méthode à utiliser pour renvoyer le jeton résultant à votre application. La valeur par défaut est `fragment`.| 
|state|facultatif|Valeur incluse dans la requête, qui est également retournée dans la réponse de jeton. Il peut s’agir d’une chaîne de tout contenu que vous souhaitez. Une valeur unique générée de manière aléatoire est généralement utilisée pour empêcher les attaques par falsification de requêtes intersites. Le paramètre state sert également à encoder les informations sur l’état de l’utilisateur dans l’application avant la requête d’authentification, comme la page ou la vue sur laquelle il se trouvait.| 
|prompt|facultatif|Indique le type d’interaction utilisateur qui est requise. Les seules valeurs valides pour l’instant sont login et none.</br>- `prompt=login` forcera l’utilisateur à entrer ses informations d’identification dans cette requête, annulant l’authentification unique. </br>- `prompt=none` est l’inverse. Cela permet de s’assurer que l’utilisateur ne reçoit aucune invite interactive. Si la requête ne peut pas être effectuée en mode silencieux par le biais de l’authentification unique, AD FS retourne une erreur interaction_required.| 
|login_hint|facultatif|Peut être utilisé pour préremplir le champ Nom d’utilisateur/Adresse e-mail de la page de connexion pour l’utilisateur, si vous connaissez son nom d’utilisateur à l’avance. Souvent, les applications utilisent ce paramètre lors de la réauthentification, après avoir extrait le nom d’utilisateur d’une connexion précédente à l’aide de la revendication `upn` à partir de `id_token`.| 
|domain_hint|facultatif|Si ce paramètre est inclus, le processus de découverte basé sur le domaine appliqué pour l’utilisateur dans la page de connexion est ignoré, ce qui se traduit par une expérience utilisateur légèrement plus fluide.| 

À ce stade, l’utilisateur sera invité à entrer ses informations d’identification et à terminer l’authentification. Une fois que l’utilisateur s’est authentifié, le point de terminaison d’autorisation AD FS retournera une réponse à votre application, au redirect_uri indiqué, à l’aide de la méthode spécifiée dans le paramètre response_mode.  
 
### <a name="successful-response"></a>Réponse positive 
 
Une réponse positive à l’aide de `response_mode=fragment and response_type=id_token+token` se présente comme suit :  
 
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
|token_type|Inclus si response_type inclut `token`. Sera toujours Bearer.| 
|expires_in| Inclus si response_type inclut `token`. Indique le nombre de secondes pendant lesquelles le jeton est valide, à des fins de mise en cache.| 
|scope| Indique la ou les étendues pour lesquelles access_token est valide.|  
|id_token|Inclus si response_type inclut `id_token`. Il s’agit d’un jeton web JSON (JWT) signé. L’application peut décoder les segments de ce jeton pour demander des informations sur l’utilisateur qui s’est connecté. L’application peut mettre en cache les valeurs et les afficher, mais elle ne doit pas se reposer sur elles pour les limites d’autorisation ou de sécurité.| 
|state|Si un paramètre state est inclus dans la requête, la même valeur doit apparaître dans la réponse. L’application doit vérifier que les valeurs state de la requête et de la réponse sont identiques.|

### <a name="refresh-tokens"></a>Jetons d’actualisation 
L’octroi implicite ne fournit pas de jetons d’actualisation.  `id_tokens` et `access_tokens` expirent après un bref laps de temps ; votre application doit donc être prête à actualiser ces jetons régulièrement. Pour actualiser l’un ou l’autre type de jeton, vous pouvez exécuter la même requête iframe masquée que ci-dessus en utilisant le paramètre `prompt=none` pour contrôler le comportement de la plateforme d’identité. Si vous souhaitez recevoir un `new id_token`, veillez à utiliser `response_type=id_token`. 

## <a name="authorization-code-grant-flow"></a>Flux d’octroi de code d’autorisation 
 
Vous pouvez utiliser l’octroi du code d’autorisation OAuth 2.0 dans les applications web pour accéder à des ressources protégées, telles que des API web. Le flux de code d’autorisation OAuth 2.0 est décrit dans la [section 4.1 de la spécification OAuth 2.0](https://tools.ietf.org/html/rfc6749). Il est utilisé pour effectuer l’authentification et l’autorisation dans la plupart des types d’applications, notamment les applications web et les applications installées en mode natif. Le flux permet aux applications d’acquérir en toute sécurité des access_tokens qui peuvent être utilisés pour accéder à des ressources qui approuvent AD FS.  
 
### <a name="protocol-diagram"></a>Diagramme de protocole 
 
À un niveau élevé, le flux d’authentification pour une application native ressemble un peu à ce qui suit :

![Flux d’octroi de code d’autorisation](media/adfs-scenarios-for-developers/authorization.png)

### <a name="request-an-authorization-code"></a>Demander un code d’autorisation 
 
Le flux de code d’autorisation commence par le client qui dirige l’utilisateur vers le point de terminaison /authorize. Dans cette requête, le client indique les autorisations qu’il doit obtenir de la part de l’utilisateur : 
 
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
|client_id|obligatoire|ID d’application (client) qu’AD FS a affecté à votre application.|  
|response_type|obligatoire| Doit inclure du code pour le flux de code d’autorisation.| 
|redirect_uri|obligatoire|`redirect_uri` de votre application, où les réponses d’authentification peuvent être envoyées et reçues par votre application. Doit correspondre exactement à l’un des redirect_uris que vous avez inscrits dans AD FS pour le client.|  
|resource|facultatif|URL de votre API web.</br>Remarque : Si vous utilisez la bibliothèque de client MSAL, le paramètre de ressource n’est pas envoyé. Au lieu de cela, l’URL de la ressource est envoyée dans le cadre du paramètre scope : `scope = [resource url]//[scope values e.g., openid]`</br>Si la ressource n’est pas passée ici ou dans l’étendue, ADFS utilise une ressource par défaut urn:microsoft:userinfo. Les stratégies de ressources userinfo, telles que MFA, Émission ou stratégie d’autorisation, ne peuvent pas être personnalisées.| 
|scope|facultatif|Liste d’étendues séparées par des espaces.|
|response_mode|facultatif|Spécifie la méthode à utiliser pour renvoyer le jeton résultant à votre application. Il peut s’agir de l’un des éléments suivants : </br>- query </br>- fragment </br>- form_post</br>`query` fournit le code en tant que paramètre de chaîne de requête sur votre URI de redirection. Si vous demandez le code, vous pouvez utiliser query, fragment ou form_post. `form_post` exécute une requête POST contenant le code de votre URI de redirection.|
|state|facultatif|Valeur incluse dans la requête, qui est également retournée dans la réponse de jeton. Il peut s’agir d’une chaîne de tout contenu que vous souhaitez. Une valeur unique générée de manière aléatoire est généralement utilisée pour empêcher les attaques par falsification de requêtes intersites. La valeur peut également encoder les informations sur l’état de l’utilisateur dans l’application avant la requête d’authentification, comme la page ou la vue sur laquelle il se trouvait.|
|prompt|facultatif|Indique le type d’interaction utilisateur qui est requise. Les seules valeurs valides pour l’instant sont login et none.</br>- `prompt=login` forcera l’utilisateur à entrer ses informations d’identification dans cette requête, annulant l’authentification unique. </br>- `prompt=none` est l’inverse. Cela permet de s’assurer que l’utilisateur ne reçoit aucune invite interactive. Si la requête ne peut pas être effectuée en mode silencieux par le biais de l’authentification unique, AD FS retourne une erreur interaction_required.|
|login_hint|facultatif|Peut être utilisé pour préremplir le champ Nom d’utilisateur/Adresse e-mail de la page de connexion pour l’utilisateur, si vous connaissez son nom d’utilisateur à l’avance. Souvent, les applications utilisent ce paramètre lors de la réauthentification, après avoir extrait le nom d’utilisateur d’une connexion précédente à l’aide de la revendication `upn`à partir de `id_token`.|
|domain_hint|facultatif|Si ce paramètre est inclus, le processus de découverte basé sur le domaine appliqué pour l’utilisateur dans la page de connexion est ignoré, ce qui se traduit par une expérience utilisateur légèrement plus fluide.|
|code_challenge_method|facultatif|Méthode utilisée pour encoder le code_verifier pour le paramètre code_challenge. Peut avoir l’une des valeurs suivantes : </br>- plain </br>- S256 </br>S’il est absent, code_challenge est supposé être en texte brut si `code_challenge` est inclus. AD FS prend en charge les méthodes plain et S256. Pour plus d’informations, consultez la [RFC PKCE](https://tools.ietf.org/html/rfc7636).|
|code_challenge|facultatif| Sert à sécuriser les octrois de code d’autorisation par le biais de PKCE (Proof Key for Code Exchange) à partir d’un client natif. Obligatoire si `code_challenge_method` est inclus. Pour plus d’informations, consultez la [RFC PKCE](https://tools.ietf.org/html/rfc7636).|

À ce stade, l’utilisateur sera invité à entrer ses informations d’identification et à terminer l’authentification. Une fois que l’utilisateur s’est authentifié, AD FS retournera une réponse à votre application, au `redirect_uri` indiqué, à l’aide de la méthode spécifiée dans le paramètre `response_mode`.  
 
### <a name="successful-response"></a>Réponse positive 
 
Une réponse positive à l’aide de response_mode=query ressemble à ceci : 
 
```
GET https://adfs.contoso.com/common/oauth2/nativeclient? 
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq... 
&state=12345  
```


|Paramètre|Description|
|-----|-----|
|code|`authorization_code` que l’application a demandé. L’application peut utiliser le code d’autorisation afin de demander un jeton d’accès pour la ressource cible. Les authorization_codes ont une courte durée de vie ; ils expirent en général après une dizaine de minutes.|
|state|Si un paramètre `state` est inclus dans la requête, la même valeur doit apparaître dans la réponse. L’application doit vérifier que les valeurs state de la requête et de la réponse sont identiques.|

### <a name="request-an-access-token"></a>Demander un jeton d’accès 
 
Maintenant que vous avez acquis un `authorization_code` et que l’utilisateur vous a octroyé une autorisation, vous pouvez échanger le code contre un `access_token` à la ressource souhaitée. Pour ce faire, envoyez une requête POST au point de terminaison /token :  
 
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
|client_id|obligatoire|ID d’application (client) qu’AD FS a affecté à votre application.| 
|grant_type|obligatoire|Doit être `authorization_code` pour le flux de code d’autorisation.| 
|code|obligatoire|`authorization_code` que vous avez acquis lors de la première phase du flux.| 
|redirect_uri|obligatoire|Même valeur `redirect_uri` que celle utilisée pour acquérir l’`authorization_code`.| 
|client_secret|obligatoire pour les applications web|Il s’agit du secret d’application que vous avez créé lors de l’inscription de l’application dans AD FS. Vous ne devez pas utiliser le secret d’application dans une application native, car les client_secrets ne peuvent pas être stockés de manière fiable sur des appareils. Il est obligatoire pour les applications web et les API web, qui ont la capacité à stocker le client_secret de façon sécurisée côté serveur. La clé secrète client doit être encodée sous forme d’URL avant d’être envoyée. Ces applications peuvent également utiliser l’authentification basée sur des clés en signant un jeton JWT et en l’ajoutant en tant que paramètre client_assertion.| 
|code_verifier|facultatif|Même `code_verifier` que celui utilisé pour obtenir l’authorization_code. Obligatoire si PKCE a été utilisé dans la requête d’octroi de code d’autorisation. Pour plus d’informations, consultez la [RFC PKCE](https://tools.ietf.org/html/rfc7636).</br>Remarque : S’applique à AD FS 2019 et versions ultérieures| 

### <a name="successful-response"></a>Réponse positive 
 
Une réponse de jeton positive se présente comme suit : 
 
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
|access_token|Jeton d’accès demandé. L’application peut utiliser ce jeton pour s’authentifier auprès de la ressource sécurisée (API web).| 
|token_type|Indique la valeur du type de jeton. Le seul type pris en charge par AD FS est Bearer.
|expires_in|Durée de validité du jeton d’accès (en secondes).
|refresh_token|Jeton d’actualisation OAuth 2.0. L’application peut utiliser ce jeton pour acquérir des jetons d’accès supplémentaires après l’expiration du jeton d’accès actif. Les refresh_tokens ont une longue durée de vie, et peuvent être utilisés pour maintenir l’accès aux ressources pendant des périodes prolongées.| 
|refresh_token_expires_in|Durée de validité du jeton d’actualisation (en secondes).| 
|id_token|Jeton web JSON (JWT). L’application peut décoder les segments de ce jeton pour demander des informations sur l’utilisateur qui s’est connecté. L’application peut mettre en cache les valeurs et les afficher, mais elle ne doit pas se reposer sur elles pour les limites d’autorisation ou de sécurité.|

### <a name="use-the-access-token"></a>Utiliser le jeton d’accès 
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q... 
 ```

### <a name="refresh-the-access-token"></a>Actualiser le jeton d’accès 
 
Les access_tokens ont une courte durée de vie, et vous devez les actualiser une fois qu’ils ont expiré pour continuer à accéder aux ressources. Pour ce faire, vous devez envoyer une autre requête POST au point de terminaison `/token`, cette fois ci en fournissant le refresh_token plutôt que le code. Les jetons d’actualisation sont valides pour toutes les autorisations pour lesquelles votre client a déjà reçu un jeton d’accès. 
 
Les jetons d’actualisation n’ont pas de durée de vie spécifiée. En règle générale, les durées de vie des jetons d’actualisation sont relativement longues. Toutefois, dans certains cas ils expirent, sont révoqués ou ne disposent pas de privilèges suffisants pour l’action souhaitée. Votre application doit prévoir et gérer correctement les erreurs retournées par le point de terminaison d’émission de jeton.  
 
Bien que les jetons d’actualisation ne soient pas révoqués quand ils sont utilisés pour acquérir de nouveaux jetons d’accès, vous êtes censé supprimer l’ancien jeton d’actualisation. La spécification OAuth 2.0 stipule que : « Le serveur d’autorisation PEUT émettre un nouveau jeton d’actualisation, auquel cas le client DOIT supprimer l’ancien jeton d’actualisation et le remplacer par le nouveau jeton d’actualisation. Le serveur d’autorisation PEUT révoquer l’ancien jeton d’actualisation après l’émission d’un nouveau jeton d’actualisation au client. » 
 
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
|client_id|obligatoire|ID d’application (client) qu’AD FS a affecté à votre application.| 
|grant_type|obligatoire|Doit être `refresh_token` pour cette phase du flux de code d’autorisation.| 
|resource|facultatif|URL de votre API web.</br>Remarque : Si vous utilisez la bibliothèque de client MSAL, le paramètre de ressource n’est pas envoyé. Au lieu de cela, l’URL de la ressource est envoyée dans le cadre du paramètre scope : `scope = [resource url]//[scope values e.g., openid]`</br>Si la ressource n’est pas passée ici ou dans l’étendue, ADFS utilise une ressource par défaut urn:microsoft:userinfo. Les stratégies de ressources userinfo, telles que MFA, Émission ou stratégie d’autorisation, ne peuvent pas être personnalisées.|
|scope|facultatif|Liste d’étendues séparées par des espaces.| 
|refresh_token|obligatoire|refresh_token que vous avez acquis lors de la deuxième phase du flux.| 
|client_secret|obligatoire pour les applications web| Secret d’application que vous avez créé dans le portail d’inscription des applications pour votre application. Vous ne devez pas l’utiliser dans une application native, car les client_secrets ne peuvent pas être stockés de manière fiable sur des appareils. Il est obligatoire pour les applications web et les API web, qui ont la capacité à stocker le client_secret de façon sécurisée côté serveur. Ces applications peuvent également utiliser l’authentification basée sur des clés en signant un jeton JWT et en l’ajoutant en tant que paramètre client_assertion.|

### <a name="successful-response"></a>Réponse positive 
Une réponse de jeton positive se présente comme suit : 
 
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
|access_token|Jeton d’accès demandé. L’application peut utiliser ce jeton pour s’authentifier auprès de la ressource sécurisée, telle qu’une API web.| 
|token_type|Indique la valeur du type de jeton. Le seul type pris en charge par AD FS est Bearer.|
|expires_in|Durée de validité du jeton d’accès (en secondes).|
|scope|Étendues pour lesquelles l’access_token est valide.| 
|refresh_token|Jeton d’actualisation OAuth 2.0. L’application peut utiliser ce jeton pour acquérir des jetons d’accès supplémentaires après l’expiration du jeton d’accès actif. Les refresh_tokens ont une longue durée de vie, et peuvent être utilisés pour maintenir l’accès aux ressources pendant des périodes prolongées.| 
|refresh_token_expires_in|Durée de validité du jeton d’actualisation (en secondes).| 
|id_token|Jeton web JSON (JWT). L’application peut décoder les segments de ce jeton pour demander des informations sur l’utilisateur qui s’est connecté. L’application peut mettre en cache les valeurs et les afficher, mais elle ne doit pas se reposer sur elles pour les limites d’autorisation ou de sécurité.|

## <a name="on-behalf-of-flow"></a>Flux On-Behalf-Of 
 
Le flux OBO (On-Behalf-Of) OAuth 2.0 est utilisé dans le cas d’usage où une application appelle un service ou une API web, qui à son tour doit appeler un autre service/API web. L’idée est de propager l’identité de l’utilisateur déléguée et les autorisations dans la chaîne de requête. Pour que le service de niveau intermédiaire puisse effectuer des requêtes authentifiées auprès du service en aval, il doit sécuriser un jeton d’accès AD FS pour le compte de l’utilisateur.  
 
### <a name="protocol-diagram"></a>Diagramme de protocole 
Supposez que l’utilisateur a été authentifié sur une application à l’aide du processus d’octroi de code d’autorisation OAuth 2.0 décrit ci-dessus. À ce stade, l’application dispose d’un jeton d’accès pour l’API A (jeton A) avec les revendications et le consentement de l’utilisateur pour accéder à l’API web de niveau intermédiaire (API A). Assurez-vous que le client demande l’étendue user_impersonation dans le jeton. Désormais, l’API A doit effectuer une requête authentifiée auprès de l’API web en aval (API B). 

Les étapes qui suivent constituent le flux OBO et sont expliquées avec l’aide du diagramme suivant. 

![Flux On-Behalf-Of](media/adfs-scenarios-for-developers/obo.png)

  1. L’application cliente envoie une requête à l’API A avec le jeton A.  
  Remarque : Lors de la configuration du flux OBO dans AD FS, assurez-vous que l’étendue `user_impersonation` est sélectionnée et que le client demande bien l’étendue `user_impersonation` dans la requête. 
  2. L’API A s’authentifie auprès du point de terminaison d’émission de jetons AD FS et demande un jeton pour accéder à l’API B. Remarque : Lors de la configuration de ce flux dans AD FS, veillez à ce que l’API A soit également inscrite en tant qu’application serveur avec un clientID ayant la même valeur que l’ID de ressource dans l’API A.
  3. Le point de terminaison d’émission de jetons AD FS valide les informations d’identification de l’API A avec le jeton A, et émet le jeton d’accès pour l’API B (jeton B). 
  4. Le jeton B est défini dans l’en-tête d’autorisation de la requête à l’API B. 
  5. Les données de la ressource sécurisée sont retournées par l’API B. 

### <a name="service-to-service-access-token-request"></a>Requête de jeton d’accès de service à service 
 
Pour demander un jeton d’accès, exécutez une requête POST HTTP sur le point de terminaison de jeton AD FS avec les paramètres suivants.  


### <a name="first-case-access-token-request-with-a-shared-secret"></a>Premier cas : requête de jeton d’accès avec un secret partagé 
 
Lors de l’utilisation d’un secret partagé, une requête de jeton d’accès de service à service contient les paramètres suivants : 


|Paramètre|Obligatoire ou facultatif|Description|
|-----|-----|-----| 
|grant_type|obligatoire|Type de requête de jeton. Pour une requête utilisant un jeton JWT, la valeur doit être urn:ietf:params:oauth:grant-type:jwt-bearer.|  
|client_id|obligatoire|ID client que vous configurez lors de l’inscription de votre première API web en tant qu’application serveur (application de niveau intermédiaire). Il doit être identique à l’ID de ressource utilisé lors de la première phase, autrement dit correspondre à l’URL de la première API web.| 
|client_secret|obligatoire|Secret d’application que vous avez créé lors de l’inscription de l’application serveur dans AD FS.| 
|assertion|obligatoire|Valeur du jeton utilisé dans la requête.|  
|requested_token_use|obligatoire|Spécifie comment la requête doit être traitée. Dans le flux OBO, ce paramètre doit avoir la valeur on_behalf_of.| 
|resource|obligatoire|ID de ressource fourni lors de l’inscription de la première API web en tant qu’application serveur (application de niveau intermédiaire). L’ID de ressource doit être l’URL de la deuxième API web que l’application de niveau intermédiaire appellera pour le compte du client.|
|scope|facultatif|Liste d’étendues séparées par des espaces pour la requête de jeton.| 

#### <a name="example"></a>Exemple 
 
La requête `HTTP POST` suivante demande un jeton d’accès et un jeton d’actualisation. 
 
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

### <a name="second-case-access-token-request-with-a-certificate"></a>Deuxième cas : requête de jeton d’accès avec un certificat 
 
Une requête de jeton d’accès de service à service avec un certificat contient les paramètres suivants : 


|Paramètre|Obligatoire ou facultatif|Description|
|-----|-----|-----| 
|grant_type|obligatoire|Type de requête de jeton. Pour une requête utilisant un jeton JWT, la valeur doit être urn:ietf:params:oauth:grant-type:jwt-bearer. |
|client_id|obligatoire|ID client que vous configurez lors de l’inscription de votre première API web en tant qu’application serveur (application de niveau intermédiaire). Il doit être identique à l’ID de ressource utilisé lors de la première phase, autrement dit correspondre à l’URL de la première API web.|  
|client_assertion_type|obligatoire|La valeur doit être urn:ietf:params:oauth:client-assertion-type:jwt-bearer.| 
|client_assertion|obligatoire|Assertion (jeton web JSON) que vous devez créer et signer avec le certificat que vous avez inscrit en tant qu’informations d’identification pour votre application.|  
|assertion|obligatoire|Valeur du jeton utilisé dans la requête.| 
|requested_token_use|obligatoire|Spécifie comment la requête doit être traitée. Dans le flux OBO, ce paramètre doit avoir la valeur on_behalf_of.| 
|resource|obligatoire|ID de ressource fourni lors de l’inscription de la première API web en tant qu’application serveur (application de niveau intermédiaire). L’ID de ressource doit être l’URL de la deuxième API web que l’application de niveau intermédiaire appellera pour le compte du client.|
|scope|facultatif|Liste d’étendues séparées par des espaces pour la requête de jeton.|


Notez que les paramètres sont presque les mêmes que dans le cas de la requête par secret partagé, sauf que le paramètre client_secret est remplacé par deux paramètres : client_assertion_type et client_assertion. 

#### <a name="example"></a>Exemple 
La requête POST HTTP suivante demande un jeton d’accès pour l’API web avec un certificat.

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
 
Une réponse positive est une réponse JSON OAuth 2.0 avec les paramètres suivants. 


|Paramètre|Description|
|-----|-----| 
|token_type|Indique la valeur du type de jeton. Le seul type pris en charge par AD FS est Bearer. | 
|scope|Étendue de l’accès accordé dans le jeton.| 
|expires_in|Durée, en secondes, pendant laquelle le jeton d’accès est valide.| 
|access_token|Jeton d’accès demandé. Le service appelant peut utiliser ce jeton pour s’authentifier auprès du service cible.| 
|id_token|Jeton web JSON (JWT). L’application peut décoder les segments de ce jeton pour demander des informations sur l’utilisateur qui s’est connecté. L’application peut mettre en cache les valeurs et les afficher, mais elle ne doit pas se reposer sur elles pour les limites d’autorisation ou de sécurité.| 
|refresh_token|Jeton d’actualisation pour le jeton d’accès demandé. Le service appelant peut utiliser ce jeton pour demander un autre jeton d’accès après l’expiration du jeton d’accès actif.|
|refresh_token_expires_in|Durée, en secondes, pendant laquelle le jeton d’actualisation est valide. 

### <a name="success-response-example"></a>Exemple de réponse positive 
 
L’exemple suivant montre une réponse positive à une requête de jeton d’accès pour l’API web. 

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
 
 
Utilisez le jeton d’accès pour accéder à la ressource sécurisée. Maintenant, le service de niveau intermédiaire peut utiliser le jeton acquis ci-dessus pour effectuer des requêtes authentifiées à l’API web en aval, en définissant le jeton dans l’en-tête d’autorisation.  

#### <a name="example"></a>Exemple 
``` 
GET /v1.0/me HTTP/1.1 
Host: https://secondwebapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQ… 
``` 

## <a name="client-credentials-grant-flow"></a>Flux d’octroi des informations d’identification du client 
 
Vous pouvez utiliser l’octroi d’informations d’identification du client OAuth 2.0 spécifié dans la [RFC 6749](https://tools.ietf.org/html/rfc6749#section-4.4) pour accéder à des ressources hébergées sur le web à l’aide de l’identité d’une application. Ce type d’octroi est couramment utilisé pour les interactions de serveur à serveur qui doivent s’exécuter en arrière-plan, sans interaction immédiate avec un utilisateur. Ces types d’applications sont souvent appelés démons ou comptes de service. 

Le flux d’octroi d’informations d’identification du client OAuth 2.0 permet à un service web (un client confidentiel) d’utiliser ses propres informations d’identification, au lieu d’emprunter l’identité d’un utilisateur, pour s’authentifier lors de l’appel d’un autre service web. Dans ce scénario, le client est généralement un service web de niveau intermédiaire, un service démon ou un site web. Pour un niveau d’assurance plus élevé, AD FS autorise également le service appelant à utiliser un certificat (plutôt qu’un secret partagé) comme informations d’identification. 

### <a name="protocol-diagram"></a>Diagramme de protocole 

Le diagramme suivant illustre le flux d’octroi des informations d’identification du client. 

![Informations d’identification du client](media/adfs-scenarios-for-developers/credentials.png)

### <a name="request-a-token"></a>Demander un jeton 
 
Pour obtenir un jeton à l’aide de l’octroi des informations d’identification du client, envoyez une requête `POST` au point de terminaison AD FS /token :  
 
### <a name="first-case-access-token-request-with-a-shared-secret"></a>Premier cas : requête de jeton d’accès avec un secret partagé 
 
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
|client_id|obligatoire|ID d’application (client) qu’AD FS a affecté à votre application.| 
|scope|facultatif|Liste des étendues, séparées par des espaces, auxquelles l’utilisateur doit donner son consentement.| 
|client_secret|obligatoire|Clé secrète client que vous avez générée pour votre application dans le portail d’inscription des applications. La clé secrète client doit être encodée sous forme d’URL avant d’être envoyée.| 
|grant_type|obligatoire|Doit avoir la valeur `client_credentials`.|

### <a name="second-case-access-token-request-with-a-certificate"></a>Deuxième cas : requête de jeton d’accès avec un certificat 

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
|client_assertion_type|obligatoire|La valeur doit être urn:ietf:params:oauth:client-assertion-type:jwt-bearer.| 
|client_assertion|obligatoire|Assertion (jeton web JSON) que vous devez créer et signer avec le certificat que vous avez inscrit en tant qu’informations d’identification pour votre application.|  
|grant_type|obligatoire|Doit avoir la valeur `client_credentials`.|
|client_id|facultatif|ID d’application (client) qu’AD FS a affecté à votre application. Fait partie de client_assertion. Il n’est donc pas nécessaire de le transmettre ici.| 
|scope|facultatif|Liste des étendues, séparées par des espaces, auxquelles l’utilisateur doit donner son consentement.| 

### <a name="use-a-token"></a>Utiliser un jeton 
 
Maintenant que vous avez acquis un jeton, utilisez-le pour exécuter des requêtes sur la ressource. Quand le jeton expire, répétez la requête sur le point de terminaison /token pour obtenir un nouveau jeton d’accès.  
 
```
GET /v1.0/me/messages 
Host: https://webapi.com 
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...  
```

## <a name="resource-owner-password-credentials-grant-flow-not-recommended"></a>Flux d’octroi des informations d’identification du mot de passe du propriétaire de la ressource (non recommandé) 
 
L’octroi des informations d’identification du mot de passe du propriétaire de la ressource (ROPC) permet à une application de connecter l’utilisateur en gérant directement son mot de passe. Le flux ROPC nécessite un degré élevé de confiance et d’exposition de l’utilisateur. Vous ne devez utiliser ce flux que quand d’autres flux, plus sécurisés, ne peuvent pas être utilisés.  
 
### <a name="protocol-diagram"></a>Diagramme de protocole 
 
Le diagramme suivant illustre le flux ROPC.

![Flux ROPC](media/adfs-scenarios-for-developers/resource.png)

### <a name="authorization-request"></a>Requête d’autorisation 
Le flux ROPC est constitué d’une requête unique : il envoie l’identification du client et les informations d’identification de l’utilisateur au fournisseur d’identité, puis reçoit les jetons en retour. Le client doit demander l’adresse e-mail de l’utilisateur (UPN) et le mot de passe avant de procéder. Immédiatement après une requête réussie, le client doit libérer de la mémoire, de manière sécurisée, les informations d’identification de l’utilisateur. Il ne doit jamais les enregistrer.  

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
|client_id|obligatoire|ID de client| 
|grant_type|obligatoire|Doit être défini sur password.| 
|username|obligatoire|Adresse de messagerie de l’utilisateur.| 
|password|obligatoire|Mot de passe de l’utilisateur.| 
|scope|facultatif|Liste d’étendues séparées par des espaces.|

### <a name="successful-authentication-response"></a>Réponse d’authentification positive 
L’exemple suivant montre une réponse de jeton positive : 

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
|scope|Si un jeton d’accès a été retourné, ce paramètre liste les étendues pour lesquelles le jeton d’accès est valide.| 
|expires_in|Nombre de secondes pendant lesquelles le jeton d’accès inclus est valide.| 
|access_token|Émis pour les étendues qui ont été demandées.| 
|id_token|Jeton web JSON (JWT). L’application peut décoder les segments de ce jeton pour demander des informations sur l’utilisateur qui s’est connecté. L’application peut mettre en cache les valeurs et les afficher, mais elle ne doit pas se reposer sur elles pour les limites d’autorisation ou de sécurité.| 
|refresh_token_expires_in|Nombre de secondes pendant lesquelles le jeton d’actualisation inclus est valide.| 
|refresh_token|Émis si le paramètre d’étendue d’origine incluait offline_access.|

Vous pouvez utiliser le jeton d’actualisation pour acquérir de nouveaux jetons d’accès et jetons d’actualisation en appliquant le même processus que celui décrit dans la section relative au flux d’octroi de code d’authentification ci-dessus.   

## <a name="device-code-flow"></a>Flux de code d’appareil 
 
L’octroi de code d’appareil permet aux utilisateurs de se connecter à des appareils avec restriction d’entrée, tels qu’une imprimante, un appareil IoT ou un téléviseur intelligent. Pour activer ce flux, l’appareil fait en sorte que l’utilisateur accède à une page web dans son navigateur sur un autre  appareil pour se connecter. Une fois que l’utilisateur s’est connecté, l’appareil peut recevoir des jetons d’accès et des jetons d’actualisation en fonction des besoins. 
 
### <a name="protocol-diagram"></a>Diagramme de protocole 
 
Le diagramme suivant illustre l’intégralité du flux de code d’appareil. Chacune des étapes est décrite plus loin dans cet article. 
 
![Flux de code d’appareil](media/adfs-scenarios-for-developers/device.png)

### <a name="device-authorization-request"></a>Requête d’autorisation de l’appareil 
Le client doit d’abord demander au serveur d’authentification un appareil et un code utilisateur utilisés pour lancer l’authentification. Le client collecte cette requête à partir du point de terminaison /devicecode. Dans cette requête, le client doit également inclure les autorisations qu’il doit obtenir de la part de l’utilisateur. À partir du moment où cette requête est envoyée, l’utilisateur ne dispose que de 15 minutes pour se connecter (la valeur habituelle du paramètre expires_in). Vous ne devez donc effectuer cette requête que quand l’utilisateur a indiqué qu’il est prêt à se connecter. 

```
// Line breaks are for legibility only. 
 
POST https://adfs.contoso.com/adfs/oauth2/devicecode 
Content-Type: application/x-www-form-urlencoded 
 
client_id=6731de76-14a6-49ae-97bc-6eba6914391e 
scope=openid 
```


|Paramètre|Condition|Description|
|-----|-----|-----| 
|client_id|obligatoire|ID d’application (client) qu’AD FS a affecté à votre application.| 
|scope|facultatif|Liste d’étendues séparées par des espaces.|

### <a name="device-authorization-response"></a>Réponse d’autorisation de l’appareil 
Une réponse positive est un objet JSON contenant les informations requises pour permettre à l’utilisateur de se connecter. 


|Paramètre|Description|
|-----|-----| 
|device_code|Chaîne longue servant à vérifier la session entre le client et le serveur d’autorisation. Le client utilise ce paramètre pour demander le jeton d’accès au serveur d’autorisation.| 
|user_code|Courte chaîne présentée à l’utilisateur, servant à identifier la session sur un appareil secondaire.| 
|verification_uri|URI auquel l’utilisateur doit accéder avec le user_code afin de se connecter.| 
|verification_uri_complete|URI auquel l’utilisateur doit accéder avec le user_code afin de se connecter. Il est prérempli avec user_code, afin que l’utilisateur n’ait pas besoin d’entrer user_code.| 
|expires_in|Nombre de secondes avant l’expiration du device_code et du user_code.| 
|interval|Nombre de secondes pendant lesquelles le client doit attendre entre les requêtes d’interrogation.| 
|message|Chaîne explicite avec des instructions destinées à l’utilisateur. Elle peut être localisée en incluant un paramètre de requête dans la requête au format ?mkt=xx-XX, en spécifiant le code de culture de langue approprié.  

### <a name="authenticating-the-user"></a>Authentification de l’utilisateur 
Une fois qu’il a reçu le user_code et le verification_uri, le client les présente à l’utilisateur, et lui demande de se connecter à l’aide du navigateur de son téléphone mobile ou de son PC. Le client peut aussi utiliser un code QR ou un mécanisme similaire pour afficher le verfication_uri_complete, afin d’effectuer l’étape d’entrée du user_code pour l’utilisateur. Pendant que l’utilisateur s’authentifie à verification_uri, le client doit interroger le point de terminaison /token pour obtenir le jeton demandé à l’aide du device_code. 

```
POST https://adfs.contoso.com /adfs/oauth2/token 
Content-Type: application/x-www-form-urlencoded 
 
grant_type: urn:ietf:params:oauth:grant-type:device_code 
client_id: 6731de76-14a6-49ae-97bc-6eba6914391e 
device_code: GMMhmHCXhWEzkobqIHGG_EnNYYsAkukHspeYUk9E8 
```


|Paramètre|obligatoire|Description|
|-----|-----|-----| 
|grant_type|obligatoire|Doit être urn:ietf:params:oauth:grant-type:device_code.| 
|client_id|obligatoire|Doit correspondre au client_id utilisé dans la requête initiale.| 
|code|obligatoire|device_code retourné dans la requête d’autorisation de l’appareil.|

### <a name="successful-authentication-response"></a>Réponse d’authentification positive 
Une réponse de jeton positive se présente comme suit :  


|Paramètre|Description|
|-----|-----| 
|token_type|Toujours Bearer.| 
|scope|Si un jeton d’accès a été retourné, ce paramètre liste les étendues pour lesquelles le jeton d’accès est valide.| 
|expires_in|Nombre de secondes pendant lesquelles le jeton d’accès inclus est valide.| 
|access_token|Émis pour les étendues qui ont été demandées.| 
|id_token|Émis si le paramètre d’étendue d’origine incluait l’étendue openid.| 
|refresh_token|Émis si le paramètre d’étendue d’origine incluait offline_access.| 
|refresh_token_expires_in|Nombre de secondes pendant lesquelles le jeton d’actualisation inclus est valide.| 


## <a name="related-content"></a>Contenu associé  
Pour obtenir la liste complète des articles de procédure pas à pas, consultez la page [Développement AD FS](../AD-FS-Development.md), qui fournit des instructions pas à pas sur l’utilisation des flux associés. 
