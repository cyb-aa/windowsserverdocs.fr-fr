---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: "Scénarios ADFS pour les développeurs"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 753b2b235cb1d73ab47588f8f229410c1f81db40
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-scenarios-for-developers"></a>Scénarios ADFS pour les développeurs

>S’applique à: Windows Server2016

AD FS dans Windows Server 2016 [AD FS 2016] vous permet ajouter industry standard connecter d’OpenID et OAuth 2.0 en fonction d’authentification et autorisation pour les applications que vous développez et avoir ces applications à authentifier les utilisateurs directement dans AD FS.    
  
AD FS 2016 prend également en charge la WS-Trust, WS-Federation et SAML protocoles et les profils de nous avoir pris en charge dans les versions précédentes.  Si vous êtes intéressé par des conseils de développement de ces protocoles, voir l’article ici.  Cet article se concentrera sur l’utilisation et de bénéficier de la prise en charge du protocole plus récente.  
  
## <a name="why-modern-authentication"></a>Pourquoi l’authentification moderne  
Pendant que vous pouvez continuer à l’aide de AD FS pour l’authentification sur avec WS-Federation, WS-Trust et les protocoles SAML exactement comme vous ont auparavant, les protocoles plus récents, vous bénéficiez des avantages suivants:  
  
* **Simplicité et la cohérence**  
    * Utilisez le même ensemble d’API et modèles pour activer l’authentification pour:   
        *   plusieurs types d’applications (serveur, bureau, mobile, navigateur)  
        *   plusieurs plateformes (android, iOS et Windows)  
        *   les applications à l’intérieur du réseau d’entreprise ou hébergé dans le cloud  
    * Utiliser le même ensemble de bibliothèques, que vous pouvez utiliser déjà pour authentifier les utilisateurs par rapport à Azure AD  
* **Flexibilité**  
    * Outre l’autorisation utilisateur standard, activer des scénarios plus complexes telles que:      
        * ? connexion à 3 branches sur les flux dans lequel un utilisateur autorise une application web ou un service pour accéder aux ressources qui se trouvent avec une autre application web ou un service.    
        * ? Flux de serveur à serveur dans lequel un service milieu accède aux API principale  
        * ? JavaScript en fonction des applications à page unique (SPA)  
* **Prise en charge**  
    * 2.0 OAuth et OpenID connecter profiter de l’utilisation dans toute l’industrie, afin de la base de connaissances de ces modèles vous permettra d’activer l’authentification et autorisation en dehors d’un environnement Active Directory ainsi  
  
## <a name="how-it-works-the-basics"></a>Fonctionnement: notions de base  
Vous pouvez ajouter l’authentification moderne à AD FS à votre application en utilisant le même ensemble d’outils et de bibliothèques, que vous pouvez utiliser déjà pour authentifier les utilisateurs par rapport à Azure AD.   
  
Dans les scénarios AD FS bien entendu, il est AD FS et pas Azure AD qui sert de fournisseur d’identité et d’autorisation serveur.  Dans le cas contraire les concepts sont identiques: les utilisateurs fournissent leurs informations d’identification et obtenir des jetons, directement ou via un intermédiaire, pour accéder aux ressources.  
  
Le scénario plus simple est constitué d’un utilisateur ou le «propriétaire de la ressource «, interaction avec un navigateur pour accéder à une application web:  
  
![AD FS pour les développeurs](media/ADFS_DEV_1.png)  
  
L’application web est appelée un «client», car il initie la requête au serveur d’autorisation (Active Directory Federation Services) pour un jeton d’accès à la ressource.  La ressource peut être hébergée par l’application web lui-même ou peut-être être accessible en tant qu’une API web quelque part sur le réseau ou internet.   L’utilisateur ou le «propriétaire de la ressource» autorise l’application web de client pour recevoir ce jeton d’accès en fournissant des informations d’identification pour le serveur d’autorisation.    
  
## <a name="how-it-works-components"></a>Fonctionnement: composants  
OAuth 2.0 OpenID connecter scénarios et marque AD FS utilisent le même ensemble d’outils et de bibliothèques lorsque Azure AD est le fournisseur d’identité.  Ces composants sont:  
* Bibliothèque d’authentification Active Directory (ADAL): les bibliothèques clientes qui facilitent collecte des informations d’identification de l’utilisateur, la création et soumettre des demandes de jeton et récupérer les jetons qui en résulte.    
* Intergiciels (middleware) OWIN (Open Interface Web pour .NET): alors que OWIN est un projet en fonction de la Communauté, Microsoft a créé un ensemble de serveur côté bibliothèques pour protéger les applications et web API avec connecter OpenID et OAuth 2.0  
  
Les rôles de ces composants sont affichés dans le diagramme ci-dessous:  
  
![AD FS pour les développeurs](media/ADFS_DEV_2.png)  
  
## <a name="modeling-these-scenarios-in-ad-fs-2016"></a>Ces scénarios de modélisation dans AD FS 2016  
  
### <a name="application-groups"></a>Groupes d’applications  
Pour représenter ces scénarios dans la stratégie d’AD FS, nous avons introduit un nouveau concept appelé des groupes d’applications.  Un groupe d’applications peut contenir n’importe quel nombre et la combinaison des types d’application fondamentaux suivants:  
  
  
  
Groupe d’applications / Type d’Application  |Description  |Rôle    
---------|---------|---------  
Application native     |  Parfois appelé un client public, ceci vise à être une application cliente qui s’exécute sur un pc ou un périphérique et avec lequel l’utilisateur interagit.       | Jetons de demandes à partir du serveur d’autorisation (Active Directory Federation Services) pour l’accès utilisateur aux ressources.  Envoie des requêtes HTTP à des ressources protégées, en utilisant les jetons comme les en-têtes HTTP.        
Application serveur     |   Une application web qui s’exécute sur un serveur et est généralement accessible aux utilisateurs via un navigateur.  Dans la mesure où il est capable de maintenir son propre secret «client» ou les informations d’identification, il est parfois appelé un client confidentiel.      | Jetons de demandes à partir du serveur d’autorisation (Active Directory Federation Services) pour l’accès utilisateur aux ressources.  Envoie des requêtes HTTP à des ressources protégées, en utilisant les jetons comme les en-têtes HTTP.               
API Web     |  Accède à la ressource de fin l’utilisateur. Vous pouvez les considérer comme la nouvelle représentation «des parties de confiance».| Utilise des jetons obtenus par les clients  
  
### <a name="differences-from-ad-fs-2012-r2"></a>Différences d’AD FS 2012 R2  
Groupes d’applications combinent des éléments de confiance et d’autorisation qui AD FS 2012 R2 exposés séparément, en tant que parties de confiance, les clients et les autorisations d’application.  
  
Le tableau suivant compare les méthodes par lequel les objets d’approbation application correspondants sont créés dans AD FS 2012 R2 vs AD FS 2016:  
  
AD FS dans Windows Server 2012 R2|Dans PowerShell|Gestion AD FS  
---------|---------|---------  
Ajouter le client natif|AdfsClient ajouter|NA  
Ajouter l’application serveur en tant que client|AdfsClient ajouter|NA  
Ajouter des API Web / ressource|Ajouter-AdfsRelyingPartyTrust|Créer la partie de confiance  
  
AD FS 2016|Dans PowerShell|Gestion AD FS  
---------|---------|---------  
Ajouter le client natif|AdfsNativeClientApplication ajouter|Ajouter un groupe d’Application à natif  
Ajouter l’application serveur en tant que client|AdfsServerApplication ajouter|Ajouter un groupe d’Application à l’Application serveur  
Ajouter des API Web / ressource|AdfsWebApiApplication ajouter|Ajouter un groupe d’Application à l’Application Web API  
  
### <a name="application-permissions-and-consent"></a>Consentement et les autorisations de l’application  
Par défaut, les clients dans un groupe d’applications sont autorisés à accéder aux ressources dans le même groupe.  L’administrateur n’a pas configurer les autorisations d’application spécifique.  Groupes d’applications permettent également aux administrateurs de spécifier les étendues autorisées, comme openid ou user_impersonation.  Les descriptions de scénario ci-dessous spécifient exactement les étendues sont requises pour le scénario.  
  
Étant donné que les services AD FS utilise un modèle d’autorisation d’administrateur, les utilisateurs ne sont pas invités à consentement lorsque l’accès aux ressources.  En configurant le groupe d’applications, l’administrateur fournit en vigueur consentement pour le compte de tous les utilisateurs de l’application.  
  
## <a name="supported-scenarios"></a>Scénarios pris en charge  
La section suivante décrit les scénarios que nous prenons en charge plus en détail.  
  
### <a name="tokens-used"></a>Jetons utilisés  
Ces scénarios de rendre l’utilisation des trois types de jeton:  
  
* **id_token:** jeton JWT A utilisé pour représenter l’identité de l’utilisateur. La revendication «aud» ou public de l’id_token correspond à l’ID de client de l’application native ou serveur.  
* **access_token:** jeton JWT A utilisé Oauth et OpenID connectent scénarios et destinés à être consommées par la ressource.  La revendication «aud» ou public de ce jeton doit correspondre à l’identificateur de la ressource ou d’une API Web.  
* **refresh_token:** ce jeton est soumis à la place de collecte des informations d’identification utilisateur pour fournir une authentification unique sur l’expérience.  Ce jeton est émis et consommé par AD FS et n’est pas lisible par les clients ou des ressources.    
  
### <a name="native-client-to-web-api"></a>API Web native client  
Ce scénario permet à l’utilisateur d’une application native client pour appeler une API de Web AD FS 2016 protégé.  
* L’application cliente natif utilise ADAL pour envoyer d’autorisation et jeton des demandes à AD FS, une demande d’informations d’identification de l’utilisateur en fonction des besoins, puis envoie le jeton obtenu en tant qu’un en-tête HTTP sur la demande à l’API Web  
* [Cette partie est uniquement à des fins de démonstration] L’API web lit les revendications à partir de l’objet ClaimsPrincipal qui provient le jeton d’accès envoyé par le client et les envoie au client.  
  
![Description du flux de protocole](media/ADFS_DEV_3.png)  
  
1.  L’application native client lance le flux avec un appel à la bibliothèque ADAL.  Cela déclenche un navigateur HTTP GET pour les services AD FS d’Autoriser le point de terminaison:  
  
**Demande d’autorisation:**  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Paramètre|Valeur  
---------|---------  
response_type|«code»  
ressource|ID RP (identificateur) de l’API Web dans le groupe d’applications  
identifiant_client|Id de l’application native dans le groupe d’applications de client  
redirect_uri|Redirection des URI d’application native dans le groupe d’applications  
  
**Réponse à la demande d’autorisation:**  
Si l’utilisateur n’a pas été signé avant, l’utilisateur est invité à fournir d’informations d’identification.    
AD FS répond en renvoyant un code d’autorisation en tant que le paramètre «code» dans le composant de requête de la redirect_uri.  Par exemple: HTTP/1.1 302 trouvé emplacement: **http://redirect_uri:80 /? code =&lt;code&gt;.**  
  
2.  Le client natif envoie ensuite le code, ainsi que les paramètres suivants, au point de terminaison jeton AD FS:  
  
**Demande de jeton:**  
POST https://fs.contoso.com/adfs/oauth2/token  
  
Paramètre|Valeur  
---------|---------  
grant_type|«authorization_code» 
code|code d’autorisation à partir de 1  
ressource|ID RP (identificateur) de l’API Web dans le groupe d’applications  
identifiant_client|Id de l’application native dans le groupe d’applications de client  
redirect_uri|Redirection des URI d’application native dans le groupe d’applications  
  
**Réponse à la demande de jeton:**  
AD FS répond avec un 200 HTTP avec l’access_token, refresh_token et id_token dans le corps.  
  
3.  L’application native puis envoie la partie access_token de la réponse ci-dessus en tant que l’en-tête d’autorisation dans la demande HTTP à l’API web.  
  
### <a name="single-sign-on-behavior"></a>Comportement d’authentification unique  
Client ultérieur demande au sein de 1 heure (par défaut) l’access_token restera valide dans le cache, et une nouvelle demande ne déclenche pas tout le trafic à AD FS.  L’access_token seront automatiquement extraites à partir du cache par ADAL.  
  
Une fois le jeton d’accès expire, ADAL automatiquement envoie une demande de basé sur JETON actualisation pour le point de terminaison jeton AD FS (en ignorant automatiquement la demande d’autorisation).  
**Actualiser la demande de jeton:**  
POST https://fs.contoso.com/adfs/oauth2/token
   

Paramètre|Valeur|
---------|---------
grant_type|«refresh_token»|
ressource|ID RP (identificateur) de l’API Web dans le groupe d’applications|
identifiant_client|Id de l’application native dans le groupe d’applications de client
refresh_token|le jeton d’actualisation émis par AD FS en réponse à la demande de jeton initiale

  
  
**Actualiser la réponse à la demande de jeton:**  
Si le jeton d’actualisation est dans < SSO_period >, la demande entraîne un nouveau jeton d’accès. L’utilisateur est invité pas d’informations d’identification.  Pour plus d’informations sur, voir paramètres SSO [AD FS Sign paramètres unique](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)  
  
Si le jeton d’actualisation a expiré, les résultats de la demande dans un HTTP 401 avec erreur «invalid_grant» et «error_description»» MSIS9615: le jeton d’actualisation reçu dans le paramètre refresh_token a expiré». Dans ce cas, ADAL envoie automatiquement une nouvelle demande d’autorisation qui ressemble à #1 ci-dessus.    
  
### <a name="web-browser-to-web-app"></a>Navigateur Web pour l’application Web   
Dans ce scénario, un utilisateur avec un navigateur doit accéder aux ressources hébergées par une application web.    
Il existe deux scénarios qui effectuent cette opération.  
  
#### <a name="oauth-confidential-client"></a>Clients confidentiels OAuth  
Ce scénario est similaire à ce qui précède dans qu’il existe une demande d’autorisation, suivie d’un code de jeton exchange.  L’application web (modélisée comme une Application de serveur dans AD FS) lance la demande d’autorisation via le navigateur et d’échange le code de jeton (en connectant directement à AD FS)  
  
![Description du flux de protocole](media/ADFS_DEV_4.png)  
  
1.  Le lance application Web d’une autorisation demande via le navigateur qui envoie un verbe HTTP GET pour les services AD FS autorise le point de terminaison  
**Demande d’autorisation**:  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Paramètre|Valeur  
---------|---------  
response_type|«code»  
ressource|ID RP (identificateur) de l’API Web dans le groupe d’applications  
identifiant_client|Id de client de l’application native dans le groupe d’applications  
redirect_uri|Redirection des URI d’application web (application serveur) dans le groupe d’applications  
  
Réponse à la demande d’autorisation:  
Si l’utilisateur n’a pas été signé avant, l’utilisateur est invité à fournir d’informations d’identification.  
AD FS répond en renvoyant un code d’autorisation en tant que le paramètre «code» dans le composant de requête de la redirect_uri, par exemple: HTTP/1.1 302 trouvé emplacement: https://webapp.contoso.com/?code=&lt;code&gt;.  
  
2.  Suite à la 302 ci-dessus, le navigateur lance un verbe HTTP GET à l’application web, par exemple: GET http://redirect_uri:80 /? code =&lt;code&gt;.   
  
3.  À ce stade l’application web, après avoir reçu le code, initie une demande à l’extrémité de jeton AD FS, envoyer les éléments suivants  
**Demande de jeton:**  
POST https://fs.contoso.com/adfs/oauth2/token  
  
Paramètre|Valeur  
---------|---------  
grant_type|«authorization_code»  
code|code d’autorisation de 2 ci-dessus  
ressource|ID RP (identificateur) de l’API Web dans le groupe d’applications  
identifiant_client|Id de client de l’application web (application serveur) dans le groupe d’applications  
redirect_uri|Redirection des URI d’application web (application serveur) dans le groupe d’applications  
client_secret|Secret de l’application web (application serveur) dans le groupe d’applications. **Remarque: Informations d’identification du client n’a pas besoin être un client_secret.  AD FS prend en charge la possibilité d’utiliser également les certificats ou l’authentification intégrée Windows.**  
  
**Réponse à la demande de jeton:**  
AD FS répond avec un 200 HTTP avec l’access_token, refresh_token et id_token dans le corps.  
revendications  
4.  Le site web application puis soit consomme la partie access_token de la réponse ci-dessus (dans le cas dans lesquels l’application web lui-même héberge la ressource), ou devez envoie en tant que l’en-tête d’autorisation dans la demande HTTP à l’API web.  
  
#### <a name="single-sign-on-behavior"></a>Comportement d’authentification unique  
Alors que le jeton d’accès toujours sera valide pour 1 heure (par défaut) dans le cache du client, vous pouvez penser que la deuxième demande fonctionnera comme dans le scénario client natif ci-dessus - qu’une nouvelle demande ne déclenche pas tout le trafic à AD FS comme le jeton d’accès est automatiquement extraites à partir du cache par ADAL.  Toutefois, il est possible que l’application web peut envoie d’autorisation distincte et demandes de jeton, liez le premier via l’URL distinct, comme dans notre exemple.  
  
Dans ce cas, il est le cookie SSO navigateur AD FS qui permet à AD FS émettre un nouveau code d’autorisation sans solliciter l’utilisateur pour les informations d’identification. L’application web s’appelle ensuite à AD FS pour échanger le nouveau code d’autorisation pour un nouveau jeton d’accès.  L’utilisateur est invité pas d’informations d’identification.  
  
Dans le cas contraire, si l’application web est actives suffisant de savoir que si l’utilisateur est déjà authentifié, la demande autoriser peut être ignorée et soit:  
* le jeton d’accès mis en cache, si vous n’a ne pas expiré, récupération et d’utilisation, ou   
* une demande en fonction de demande jeton peut être envoyée à l’extrémité jeton AD FS, comme décrit ci-dessous  
  
**Actualiser la demande de jeton:**  
POST https://fs.contoso.com/adfs/oauth2/token
   
Paramètre|Valeur  
---------|---------  
grant_type|«refresh_token»  
ressource|ID RP (identificateur) de l’API Web dans le groupe d’applications  
identifiant_client|Id de client de l’application web (application serveur) dans le groupe d’applications  
refresh_token|Actualiser les jetons émis par AD FS en réponse à la demande de jeton initiale  
client_secret|Secret de l’application web (application serveur) dans le groupe d’applications  
  
**Actualiser la réponse à la demande de jeton:**  
Si le jeton d’actualisation est dans < SSO_period >, la demande entraîne un nouveau jeton d’accès. L’utilisateur est invité pas d’informations d’identification. Pour plus d’informations sur, voir paramètres SSO [AD FS Sign paramètres unique](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)   
  
Si le jeton d’actualisation a expiré, les résultats de la demande dans un HTTP 401 avec erreur «invalid_grant» et «error_description»» MSIS9615: le jeton d’actualisation reçu dans le paramètre refresh_token a expiré». Dans ce cas, ADAL envoie automatiquement une nouvelle demande d’autorisation qui ressemble à #1 ci-dessus.    
  
#### <a name="openid-connect-hybrid-flow"></a>OpenID Connect: Flux de hybride  
Ce scénario est similaire à ce qui précède dans qui il est une demande d’autorisation lancée par l’application web via la redirection du navigateur et un code de jeton Exchange à partir de l’application web AD FS.  La différence dans ce scénario est que AD FS émet un id_token dans le cadre de la réponse à la demande initiale d’autorisation.  
  
![Description du flux de protocole](media/ADFS_DEV_5.png)  
  
1.  Le lance application Web d’une autorisation demande via le navigateur qui envoie un verbe HTTP GET pour les services AD FS autorise le point de terminaison  
  
**Demande d’autorisation:**  
GET https://fs.contoso.com/adfs/oauth2/authorize?  
  
Paramètre|Valeur  
---------|---------  
response_type|«code + id_token»  
response_mode|«form_post»  
ressource|ID RP (identificateur) de l’API Web dans le groupe d’applications  
identifiant_client|Id de client de l’application web (application serveur) dans le groupe d’applications  
redirect_uri|Redirection des URI d’application web (application serveur) dans le groupe d’applications  
  
**Réponse à la demande d’autorisation:**  
Si l’utilisateur n’a pas été signé avant, l’utilisateur est invité à fournir d’informations d’identification.  
AD FS répond avec un 200 HTTP et le formulaire contenant le ci-dessous comme masqué éléments:  
* code: le code d’autorisation  
* id_token: un jeton JWT contenant des revendications décrivant l’authentification utilisateur  
2.  Le formulaire publie automatiquement à la redirect_uri de l’application web, envoyer le code et l’id_token à l’application web.  
  
3.  À ce stade l’application web, après avoir reçu le code, initie une demande à l’extrémité de jeton AD FS, envoyer les éléments suivants  
  
**Demande de jeton:**  
POST https://fs.contoso.com/adfs/oauth2/token
  
  
  
Paramètre|Valeur  
---------|---------  
grant_type|«authorization_code»  
code|code d’autorisation à partir du haut  
ressource|ID RP (identificateur) de l’API Web dans le groupe d’applications  
identifiant_client|Id de client de l’application web (application serveur) dans le groupe d’applications  
redirect_uri|Redirection des URI d’application web (application serveur) dans le groupe d’applications  
client_secret|Secret de l’application web (application serveur) dans le groupe d’applications  
  
**Réponse à la demande de jeton:**  
AD FS répond avec un 200 HTTP avec l’access_token, refresh_token et id_token dans le corps.  
  
4.  Le site web application puis soit consomme la partie access_token de la réponse ci-dessus (dans le cas dans lesquels l’application web lui-même héberge la ressource), ou devez envoie en tant que l’en-tête d’autorisation dans la demande HTTP à l’API web.  
  
#### <a name="single-sign-on-behavior"></a>Comportement d’authentification unique  
L’authentification unique sur le comportement est identique à celle du flux de clients confidentiels Oauth 2.0 ci-dessus.  
  
### <a name="on-behalf-of"></a>De la part de  
Dans ce scénario, une application web utilise le jeton d’accès d’origine à partir d’un utilisateur pour demander et obtenir un nouveau jeton d’accès pour une autre API Web, laquelle l’application web sera ensuite accéder en tant que l’utilisateur final.  Il s’agit d’un flux «de la part de».  
  
![Description du flux de protocole](media/ADFS_DEV_6.png)  
  
Les étapes 1 et 2 fonctionnent comme les étapes 3 et 4 dans le flux précédent.  
À l’étape 3, la nécessité de clé est que le paramètre identifiant_client, l’ID de client de l’application Web 2, doit correspondre à l’ID de Web API RP A. En d’autres termes, le public le jeton d’accès en cours d’échange pour le nouveau jeton doit correspondre à l’ID de client de l’entité demandant le nouveau jeton.  

## <a name="related-content"></a>Contenu associé  
Voir [AD FS développement](../AD-FS-Development.md) pour obtenir la liste complète des articles étape par étape, qui fournissent des instructions détaillées sur l’utilisation de flux connexes. 
