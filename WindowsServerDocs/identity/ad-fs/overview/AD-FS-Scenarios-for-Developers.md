---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: Scénarios AD FS pour les développeurs
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb1bc5776ea4d24f274c79563d9e346b104de6d9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444223"
---
# <a name="ad-fs-scenarios-for-developers"></a>Scénarios AD FS pour les développeurs


AD FS dans Windows Server 2016 [AD FS 2016] vous permet de peuvent ajouter industry standard OpenID Connect et OAuth 2.0 en fonction d’authentification et autorisation aux applications que vous développez et ces applications authentifier les utilisateurs directement dans AD FS.    
  
AD FS 2016 prend également en charge le WS-Trust, WS-Federation et SAML protocoles et les profils de nous avoir pris en charge dans les versions précédentes.  Si vous vous intéressez dans le Guide du développeur pour ces protocoles, consultez l’article ici.  Cet article se concentrera sur l’utilisation et de tirer parti de la plus récente prise en charge du protocole.  
  
## <a name="why-modern-authentication"></a>Pourquoi l’authentification moderne  
Vous pouvez continuer avec AD FS pour l’authentification sur avec WS-Federation, WS-Trust et les protocoles SAML comme vous ont avant, avec les protocoles plus récente, vous bénéficiez des avantages suivants :  
  
* **Simplicité et cohérence**  
    * Utilisez le même ensemble d’API et de modèles pour activer l’authentification pour :   
        *   plusieurs types d’applications (serveur, de bureau, mobile, navigateur)  
        *   plusieurs plateformes (android, iOS, Windows)  
        *   applications à l’intérieur du réseau d’entreprise soit hébergé dans le cloud  
    * Utiliser le même ensemble de bibliothèques, que vous pouvez déjà utiliser pour authentifier les utilisateurs auprès d’Azure AD  
* **Flexibilité**  
    * En plus de l’autorisation de l’utilisateur standard, activer des scénarios plus complexes telles que :      
        * ? connexion en 3 étapes sur les flux dans lequel un utilisateur autorise une application web ou service pour accéder aux ressources qui se trouvent avec une autre application web ou service.    
        * ? Flux de serveur à serveur dans lequel un service de niveau intermédiaire accède à un backend d’API  
        * ? Applications à page unique (SPA) basé sur JavaScript  
* **Prise en charge**  
    * OAuth 2.0 et OpenID Connect profiter de l’utilisation de du secteur, afin de la base de connaissances de ces modèles vous aideront à activer l’authentification et l’autorisation en dehors d’un environnement Active Directory, ainsi  
  
## <a name="how-it-works-the-basics"></a>Fonctionnement : Les principes de base  
Vous pouvez ajouter l’authentification moderne AD FS à votre application en utilisant le même ensemble d’outils et bibliothèques que vous pouvez déjà utiliser pour authentifier les utilisateurs auprès d’Azure AD.   
  
Dans les scénarios AD FS, bien sûr, il est AD FS et pas Azure AD qui sert de fournisseur d’identité et d’autorisation serveur.  Sinon, les concepts sont exactement les mêmes : les utilisateurs fournissent leurs informations d’identification et obtenir des jetons, directement ou via un intermédiaire, pour accéder aux ressources.  
  
Le scénario plus simple se compose d’un utilisateur ou le « propriétaire de ressource », interagir avec un navigateur pour accéder à une application web :  
  
![AD FS pour les développeurs](media/ADFS_DEV_1.png)  
  
L’application web est appelée un « client », car il initie la demande au serveur d’autorisation (AD FS) pour un jeton d’accès à la ressource.  La ressource peut être hébergée par l’application web elle-même ou peut être accessible en tant qu’une API web quelque part sur le réseau ou internet.   L’utilisateur ou le « propriétaire de ressource » autorise l’application web cliente pour recevoir ce jeton d’accès en fournissant des informations d’identification pour le serveur d’autorisation.    
  
## <a name="how-it-works-components"></a>Comment cela fonctionne : composants  
OAuth 2.0 et OpenID Connect de scénarios dans Assurez-vous d’AD FS utilisent le même ensemble d’outils et bibliothèques que vous utilisez lorsque Azure AD est le fournisseur d’identité.  Ces composants sont :  
* Active Directory Authentication Library (ADAL) : les bibliothèques clientes qui facilitent collecte les informations d’identification de l’utilisateur, la création et envoi de demandes de jeton et récupérer les jetons générés.    
* Intergiciel OWIN (Open Web Interface pour .NET) : OWIN est un projet de la Communauté, Microsoft a créé un ensemble de bibliothèques de côté serveur pour la protection des applications web et API web avec OpenID Connect et OAuth 2.0  
  
Les rôles de ces composants sont affichés dans le diagramme ci-dessous :  
  
![AD FS pour les développeurs](media/ADFS_DEV_2.png)  
  
## <a name="modeling-these-scenarios-in-ad-fs-2016"></a>Ces scénarios de modélisation dans AD FS 2016  
  
### <a name="application-groups"></a>Groupes d’applications  
Pour représenter ces scénarios dans la stratégie d’AD FS, nous avons introduit un nouveau concept appelé des groupes d’applications.  Un groupe d’applications peut contenir n’importe quel nombre et la combinaison des types fondamentaux suivants de l’application :  
  
  
  
Groupe d’applications / Type d’Application  |Description  |Rôle    
---------|---------|---------  
Application native     |  Parfois appelé un client public, doit être une application cliente qui s’exécute sur un pc ou un appareil et avec lesquels l’utilisateur interagit.       | Demande de jetons à partir du serveur d’autorisation (AD FS) pour l’accès utilisateur aux ressources.  Envoie des requêtes HTTP à des ressources protégées, l’aide des jetons en tant qu’en-têtes HTTP.        
Application serveur     |   Une application web qui s’exécute sur un serveur et est généralement accessibles aux utilisateurs via un navigateur.  Comme il est capable de maintenir son propre « question secrète du client » ou les informations d’identification, il est parfois appelé un client confidentiel.      | Demande de jetons à partir du serveur d’autorisation (AD FS) pour l’accès utilisateur aux ressources.  Envoie des requêtes HTTP à des ressources protégées, l’aide des jetons en tant qu’en-têtes HTTP.               
API Web     |  Accède à la ressource de fin l’utilisateur. Vous pouvez les considérer comme la nouvelle représentation « des parties de confiance ».| Utilise des jetons obtenus par les clients  
  
### <a name="differences-from-ad-fs-2012-r2"></a>Différences par rapport à AD FS 2012 R2  
Application combinent les éléments de confiance et d’autorisation AD FS 2012 R2 exposées séparément, en tant que parties de confiance, les clients et les autorisations d’application.  
  
Les tableaux ci-dessous comparent les méthodes par lequel les objets d’approbation application correspondants sont créés dans AD FS 2012 R2 vs AD FS 2016 :  
  
AD FS dans Windows Server 2012 R2|Dans PowerShell|Gestion AD FS  
---------|---------|---------  
Ajouter un client natif|Add-AdfsClient|N/A  
Ajouter l’application de serveur en tant que client|Add-AdfsClient|N/A  
Ajouter des API Web / ressources|Add-AdfsRelyingPartyTrust|Créer la partie de confiance  
  
AD FS 2016|Dans PowerShell|Gestion AD FS  
---------|---------|---------  
Ajouter un client natif|Add-AdfsNativeClientApplication|Ajouter un groupe d’Application à l’Application Native  
Ajouter l’application de serveur en tant que client|Add-AdfsServerApplication|Ajouter un groupe à une Application de serveur  
Ajouter des API Web / ressources|Add-AdfsWebApiApplication|Ajouter un groupe d’Application à l’Application Web API  
  
### <a name="application-permissions-and-consent"></a>Consentement et les autorisations d’application  
Par défaut, les clients dans un groupe d’applications sont autorisés à accéder aux ressources dans le même groupe.  L’administrateur n’a pas configurer les autorisations d’application spécifique.  Groupes d’applications permettent également aux administrateurs de spécifier les étendues autorisées, tels qu’openid ou user_impersonation.  Les descriptions de scénario ci-dessous spécifient exactement les étendues qui sont requis pour le scénario.  
  
Étant donné que AD FS utilise un modèle de consentement de l’administrateur, les utilisateurs n’êtes pas invités à fournir son consentement lors de l’accès aux ressources.  En configurant le groupe d’applications, l’administrateur fournit en effet consentement pour le compte de tous les utilisateurs de l’application.  
  
## <a name="supported-scenarios"></a>Scénarios pris en charge  
La section suivante décrit les scénarios que nous prenons en charge plus en détail.  
  
### <a name="tokens-used"></a>Jetons utilisés  
Ces scénarios utilisent trois types de jeton :  
  
* **id_token:** Un jeton JWT est utilisé pour représenter l’identité de l’utilisateur. La revendication « aud » ou audience du jeton id_token correspond à l’ID de client de l’application native ou serveur.  
* **access_token:** Un jeton JWT utilisé dans Oauth et OpenID connectent des scénarios et destinés à être consommés par la ressource.  La revendication « aud » ou audience de ce jeton doit correspondre à l’identificateur de la ressource ou l’API Web.  
* **refresh_token:** Ce jeton est soumis à la place de la collecte des informations d’identification de l’utilisateur pour fournir une authentification unique sur l’expérience.  Ce jeton est émis et consommé par AD FS et n’est pas lisible par les clients ou des ressources.    
  
### <a name="native-client-to-web-api"></a>Native client pour l’API Web  
Ce scénario permet à l’utilisateur d’une application cliente native pour appeler une API Web de 2016 protégé par AD FS.  
* L’application cliente native utilise ADAL pour envoyer d’autorisation et le jeton de demande aux services AD FS, demande d’informations d’identification de l’utilisateur en fonction des besoins, puis envoie le jeton résultant comme en-tête HTTP sur la demande à l’API Web  
* [Cette partie est uniquement à des fins de démonstration] L’API web lit les revendications à partir de l’objet ClaimsPrincipal qui a obtenu le jeton d’accès envoyé par le client et les envoie au client.  
  
![Description du flux du protocole](media/ADFS_DEV_3.png)  
  
1.  L’application cliente native lance le flux avec un appel à la bibliothèque ADAL.  Cela déclenche un basée sur un navigateur HTTP GET pour les services AD FS d’Autoriser le point de terminaison :  
  
**Demande d’autorisation :**  
TÉLÉCHARGER <https://fs.contoso.com/adfs/oauth2/authorize?>  
  
Paramètre|Value  
---------|---------  
response_type|« code »  
resource|ID de fournisseur de ressources (identificateur) de l’API Web dans le groupe d’applications  
client_id|Id client de l’application native dans le groupe d’applications  
redirect_uri|URI de redirection de l’application native dans le groupe d’applications  
  
**Réponse de demande d’autorisation :**  
Si l’utilisateur n’a pas été signé avant que l’utilisateur est invité à entrer des informations d’identification.    
AD FS répond en retournant un code d’autorisation en tant que le paramètre « code » dans le composant de requête du redirect_uri.  Exemple : HTTP/1.1 302 trouvé l’emplacement :  **<http://redirect_uri:80/?code=&lt;code&gt>;.**  
  
2. Le client natif envoie ensuite le code, ainsi que les paramètres suivants, au point de terminaison de jeton AD FS :  
  
**Demande de jeton :**  
POST https://fs.contoso.com/adfs/oauth2/token  
  
Paramètre|Value  
---------|---------  
grant_type|"authorization_code" 
code|code d’autorisation à partir de 1  
resource|ID de fournisseur de ressources (identificateur) de l’API Web dans le groupe d’applications  
client_id|Id client de l’application native dans le groupe d’applications  
redirect_uri|URI de redirection de l’application native dans le groupe d’applications  
  
**Réponse de demande de jeton :**  
AD FS répond avec HTTP 200 avec le jeton d’accès, jeton d’actualisation et le jeton id_token dans le corps.  
  
3. L’application native puis envoie la partie du jeton d’accès de la réponse ci-dessus en tant que l’en-tête d’autorisation dans la requête HTTP à l’API web.  
  
### <a name="single-sign-on-behavior"></a>Comportement de l’authentification unique  
Suivantes du client demande au sein de 1 heure (par défaut) le jeton d’accès seront encore valides dans le cache, et une nouvelle demande ne déclenche pas tout le trafic vers AD FS.  Le jeton d’accès est automatiquement extraite à partir du cache par la bibliothèque ADAL.  
  
Une fois que le jeton d’accès expire, la bibliothèque ADAL envoie automatiquement une demande en jetons d’actualisation pour le point de terminaison jeton AD FS (ignorer la demande d’autorisation automatiquement).  
**Demande de jeton d’actualisation :**  
POST https://fs.contoso.com/adfs/oauth2/token
   

Paramètre|Value|
---------|---------
grant_type|"refresh_token"|
resource|ID de fournisseur de ressources (identificateur) de l’API Web dans le groupe d’applications|
client_id|Id client de l’application native dans le groupe d’applications
refresh_token|le jeton d’actualisation émis par AD FS en réponse à la demande de jeton initiale

  
  
**Réponse de demande de jeton d’actualisation :**  
Si le jeton d’actualisation se trouve dans < SSO_period >, la demande entraîne un nouveau jeton d’accès. L’utilisateur n’est pas invité pour les informations d’identification.  Pour plus d’informations sur les paramètres, consultez SSO [AD FS Single Sign On Settings](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)  
  
Si le jeton d’actualisation a expiré, la demande entraîne un HTTP 401 avec l’erreur « invalid_grant » et « error_description » « MSIS9615 : Le jeton d’actualisation reçu dans le paramètre de jeton d’actualisation a expiré ». Dans ce cas, la bibliothèque ADAL envoie automatiquement une nouvelle demande d’autorisation qui ressemble à #1 ci-dessus.    
  
### <a name="web-browser-to-web-app"></a>Navigateur Web à l’application Web   
Dans ce scénario, un utilisateur avec un navigateur doit accéder aux ressources hébergées par une application web.    
Il existe deux scénarios qui effectuent cette opération.  
  
#### <a name="oauth-confidential-client"></a>Client confidentiel OAuth  
Ce scénario est semblable à celle ci-dessus dans qu’il existe une demande d’autorisation, suivie d’un code pour l’échange de jeton.  Lance la demande d’autorisation via le navigateur de l’application web (modélisée comme une Application de serveur dans AD FS) et échange le code pour le jeton (en connectant directement à AD FS)  
  
![Description du flux du protocole](media/ADFS_DEV_4.png)  
  
1. Le lance d’application Web une autorisation demande via le navigateur, qui envoie une requête HTTP GET pour les services AD FS autorise le point de terminaison  
   **Demande d’autorisation**:  
   TÉLÉCHARGER <https://fs.contoso.com/adfs/oauth2/authorize?>  
  
Paramètre|Value  
---------|---------  
response_type|« code »  
resource|ID de fournisseur de ressources (identificateur) de l’API Web dans le groupe d’applications  
client_id|Id client de l’application native dans le groupe d’applications  
redirect_uri|URI de redirection de l’application web (application serveur) dans le groupe d’applications  
  
Réponse de demande d’autorisation :  
Si l’utilisateur n’a pas été signé avant que l’utilisateur est invité à entrer des informations d’identification.  
AD FS répond en retournant un code d’autorisation en tant que le paramètre « code » dans le composant de requête du redirect_uri, par exemple : HTTP/1.1 302 trouvé l’emplacement : <https://webapp.contoso.com/?code=&lt;code&gt>;.  
  
2. À la suite la 302 ci-dessus, le navigateur démarre une opération HTTP GET à l’application web, par exemple : GET <http://redirect_uri:80/?code=&lt;code&gt>;.   
  
3. À ce stade l’application web, ayant reçu le code, lance une demande d’AD FS point de terminaison token, envoyer les éléments suivants  
   **Demande de jeton :**  
   POST https://fs.contoso.com/adfs/oauth2/token  
  
Paramètre|Value  
---------|---------  
grant_type|"authorization_code"  
code|code d’autorisation à partir de 2 ci-dessus  
resource|ID de fournisseur de ressources (identificateur) de l’API Web dans le groupe d’applications  
client_id|Id de client de l’application web (application serveur) dans le groupe d’applications  
redirect_uri|URI de redirection de l’application web (application serveur) dans le groupe d’applications  
client_secret|Clé secrète de l’application web (application serveur) dans le groupe d’applications. **Remarque : Informations d’identification du client n’a pas besoin être un client_secret.  AD FS prend en charge la possibilité d’utiliser également des certificats ou l’authentification intégrée de Windows.**  
  
**Réponse de demande de jeton :**  
AD FS répond avec HTTP 200 avec le jeton d’accès, jeton d’actualisation et le jeton id_token dans le corps.  
claims  
4. Le web application puis soit consomme la partie du jeton d’accès de la réponse ci-dessus (dans le cas dans lequel l’application web elle-même héberge la ressource), ou envoie sous forme de l’en-tête d’autorisation dans la requête HTTP à l’API web.  
  
#### <a name="single-sign-on-behavior"></a>Comportement de l’authentification unique  
Bien que le jeton d’accès sera toujours valid pendant 1 heure (par défaut) dans le cache du client, vous pouvez penser que la deuxième demande fonctionnera comme dans le scénario de client natif ci-dessus - qu’une nouvelle demande ne déclenche pas tout le trafic aux services AD FS comme le jeton d’accès sera automatiquement être extraites à partir du cache par la bibliothèque ADAL.  Toutefois, il est possible que l’application web peut envoyer d’autorisation distincte et demandes de jeton, l’ancienne base de données via des liens URL distinctes, comme dans notre exemple.  
  
Dans ce cas, il est le cookie d’authentification unique de navigateur AD FS qui permet à AD FS émettre un nouveau code d’autorisation sans solliciter l’utilisateur pour les informations d’identification. L’application web appelle ensuite à AD FS pour échanger le nouveau code d’autorisation pour un nouveau jeton d’accès.  L’utilisateur n’est pas invité pour les informations d’identification.  
  
Sinon, si l’application web est actives suffisant de savoir que si l’utilisateur est déjà authentifié, la demande authorize peut être ignorée et la valeur :  
* le jeton d’accès mis en cache, si ne pas expiré, est récupéré et utilisé, ou   
* une demande d’en jeton de demande peut être envoyée au point de terminaison de jeton de AD FS, comme décrit ci-dessous  
  
**Demande de jeton d’actualisation :**  
POST https://fs.contoso.com/adfs/oauth2/token
   
Paramètre|Value  
---------|---------  
grant_type|"refresh_token"  
resource|ID de fournisseur de ressources (identificateur) de l’API Web dans le groupe d’applications  
client_id|Id de client de l’application web (application serveur) dans le groupe d’applications  
refresh_token|Actualiser le jeton émis par AD FS en réponse à la demande de jeton initiale  
client_secret|Secret de l’application web (application serveur) dans le groupe d’applications  
  
**Réponse de demande de jeton d’actualisation :**  
Si le jeton d’actualisation se trouve dans < SSO_period >, la demande entraîne un nouveau jeton d’accès. L’utilisateur n’est pas invité pour les informations d’identification. Pour plus d’informations sur les paramètres, consultez SSO [AD FS Single Sign On Settings](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)   
  
Si le jeton d’actualisation a expiré, la demande entraîne un HTTP 401 avec l’erreur « invalid_grant » et « error_description » « MSIS9615 : Le jeton d’actualisation reçu dans le paramètre de jeton d’actualisation a expiré ». Dans ce cas, la bibliothèque ADAL envoie automatiquement une nouvelle demande d’autorisation qui ressemble à #1 ci-dessus.    
  
#### <a name="openid-connect-hybrid-flow"></a>OpenID Connect : Flux hybride  
Ce scénario est semblable à celle ci-dessus dans qui il est une demande d’autorisation lancée par l’application web via la redirection du navigateur et un code pour l’échange de jeton à partir de l’application web AD FS.  La différence dans ce scénario est que AD FS émet un jeton id_token en tant que partie de la réponse de demande d’autorisation initiale.  
  
![Description du flux du protocole](media/ADFS_DEV_5.png)  
  
1.  Le lance d’application Web une autorisation demande via le navigateur, qui envoie une requête HTTP GET pour les services AD FS autorise le point de terminaison  
  
**Demande d’autorisation :**  
TÉLÉCHARGER <https://fs.contoso.com/adfs/oauth2/authorize?>  
  
Paramètre|Value  
---------|---------  
response_type|"code+id_token"  
response_mode|"form_post"  
resource|ID de fournisseur de ressources (identificateur) de l’API Web dans le groupe d’applications  
client_id|Id de client de l’application web (application serveur) dans le groupe d’applications  
redirect_uri|URI de redirection de l’application web (application serveur) dans le groupe d’applications  
  
**Réponse de demande d’autorisation :**  
Si l’utilisateur n’a pas été signé avant que l’utilisateur est invité à entrer des informations d’identification.  
AD FS répond avec un HTTP 200 et le formulaire contenant le ci-dessous comme masqué éléments :  
* code : le code d’autorisation  
* id_token : un jeton Web JSON contenant des revendications qui décrivent l’authentification utilisateur  
* Le formulaire publie automatiquement à l’URI de redirection de l’application web, en envoyant le code et le paramètre id_token à l’application web.  
  
3. À ce stade l’application web, ayant reçu le code, lance une demande d’AD FS point de terminaison token, envoyer les éléments suivants  
  
**Demande de jeton :**  
POST https://fs.contoso.com/adfs/oauth2/token
  
  
  
Paramètre|Value  
---------|---------  
grant_type|"authorization_code"  
code|code d’autorisation à partir du haut  
resource|ID de fournisseur de ressources (identificateur) de l’API Web dans le groupe d’applications  
client_id|Id de client de l’application web (application serveur) dans le groupe d’applications  
redirect_uri|URI de redirection de l’application web (application serveur) dans le groupe d’applications  
client_secret|Secret de l’application web (application serveur) dans le groupe d’applications  
  
**Réponse de demande de jeton :**  
AD FS répond avec HTTP 200 avec le jeton d’accès, jeton d’actualisation et le jeton id_token dans le corps.  
  
4. Le web application puis soit consomme la partie du jeton d’accès de la réponse ci-dessus (dans le cas dans lequel l’application web elle-même héberge la ressource), ou envoie sous forme de l’en-tête d’autorisation dans la requête HTTP à l’API web.  
  
#### <a name="single-sign-on-behavior"></a>Comportement de l’authentification unique  
La comportement de l’authentification unique est la même que pour le flux de client confidentiel Oauth 2.0 ci-dessus.  
  
### <a name="on-behalf-of"></a>Pour le compte de  
Dans ce scénario, une application web utilise le jeton d’accès d’origine à partir d’un utilisateur pour demander et d’obtenir un autre jeton d’accès pour une autre API Web, auquel l’application web sera accéder ensuite que l’utilisateur final.  Il s’agit d’un flux « de la part de ».  
  
![Description du flux du protocole](media/ADFS_DEV_6.png)  
  
Les étapes 1 et 2 fonctionnent comme étapes 3 et 4 dans le flux précédent.  
À l’étape 3, la condition principale est que le paramètre client_id, l’ID client de l’application Web 2, doit correspondre à l’ID de fournisseur de ressources de Web API A.  En d’autres termes, l’audience du jeton échangé pour le nouveau jeton d’accès doit correspondre à l’ID client de l’entité qui demande le nouveau jeton.  

## <a name="related-content"></a>Contenu associé  
Consultez [AD FS développement](../AD-FS-Development.md) pour obtenir la liste complète des articles de procédure pas à pas, qui fournissent des instructions détaillées sur l’utilisation des flux associés. 
