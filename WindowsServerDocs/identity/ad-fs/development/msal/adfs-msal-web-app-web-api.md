---
title: AD FS de l’application Web MSAL (application serveur) appelant des API Web
description: Découvrez comment créer des utilisateurs de connexion d’application Web authentifiés par AD FS 2019.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2ac36180992d44f837ce74ace40cf95533309c9
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69983427"
---
# <a name="scenario-web-app-server-app-calling-web-api"></a>Scénario : Application Web (application serveur) appelant l’API Web 
>S'applique à : AD FS 2019 et versions ultérieures 
 
Découvrez comment créer des utilisateurs de connexion d’application Web authentifiés par AD FS 2019 et acquérant des jetons à l’aide de la [bibliothèque MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) pour appeler des API Web.  
 
Avant de lire cet article, vous devez vous familiariser avec les [concepts de AD FS](../ad-fs-openid-connect-oauth-concepts.md) et le [flow d’octroi de code d’autorisation](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)
 
## <a name="overview"></a>Vue d'ensemble 
 
![Vue d’ensemble de l’application Web appelant l’API Web](media/adfs-msal-web-app-web-api/webapp1.png)

Dans ce processus, vous ajoutez l’authentification à votre application Web (application serveur), qui peut donc connecter des utilisateurs et appeler une API Web. À partir de l’application Web, pour appeler l’API Web, utilisez la méthode d’acquisition de jeton [AcquireTokenByAuthorizationCode](https://docs.microsoft.com/en-us/dotnet/api/microsoft.identity.client.acquiretokenbyauthorizationcodeparameterbuilder?view=azure-dotnet) de MSAL. Vous utiliserez le workflow de code d’autorisation, en stockant le jeton acquis dans le cache de jeton. Le contrôleur acquiert ensuite les jetons en mode silencieux à partir du cache, si nécessaire. MSAL actualise le jeton, si nécessaire. 

Web Apps qui appelle des API Web: 


- sont des applications clientes confidentielles. 
- C’est pourquoi ils ont inscrit une clé secrète (secret partagé de l’application, certificat ou compte AD) avec AD FS. Ce secret est transmis lors de l’appel à AD FS pour obtenir un jeton.  

Pour mieux comprendre comment inscrire une application Web dans ADFS et la configurer pour acquérir des jetons pour appeler une API Web, nous allons utiliser un exemple disponible [ici](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi) et suivre les étapes d’inscription et de configuration du code de l’application.  

 
## <a name="pre-requisites"></a>Prérequis 

- Outils clients GitHub 
- AD FS 2019 ou une version ultérieure configurée et en cours d’exécution 
- Visual Studio 2013 ou une version ultérieure 
 
## <a name="app-registration-in-ad-fs"></a>Inscription d’application dans AD FS 
Cette section montre comment inscrire l’application Web en tant que client confidentiel et API Web en tant que partie de confiance (RP) dans AD FS. 

  1. Dans AD FS gestion, cliquez avec le bouton droit sur **groupes d’applications** , puis sélectionnez Ajouter un **groupe d’applications**.  
  2. Dans l’Assistant groupe d’applications, pour le **nom** , entrez **WebAppToWebApi** , puis sous **applications client-serveur** , sélectionnez l' **application serveur qui accède à un modèle d’API Web** . Cliquez sur **Suivant**.  
  
      ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp2.png)
  
  3. Copiez la valeur de l' **identificateur du client** . Il sera utilisé ultérieurement comme valeur pour **Ida: ClientID** dans le fichier **Web. config** des applications. Entrez les informations suivantes pour l’URI de redirection **:**  - https://localhost:44326. Cliquez sur Ajouter. Cliquez sur **Suivant**. 
  
      ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp3.png)
  
  4. Dans l’écran configurer les informations d’identification de l’application, activez la case à cocher **générer un secret partagé** et copier le secret. Ce sera utilisé ultérieurement comme valeur pour **Ida: ClientSecret** dans le fichier **Web. config** des applications. Cliquez sur **Suivant**.  
  
      ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp4.png)
  
  5. Dans l’écran configurer l’API Web, entrez l' **identificateur:** https://webapi. Cliquez sur **Ajouter**. Cliquez sur **Suivant**. Cette valeur sera utilisée ultérieurement pour **Ida: GraphResourceId** dans le fichier **Web. config** des applications. 
  
      ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp5.png)
  
  6. Dans l’écran appliquer la stratégie de Access Control, sélectionnez **autoriser** tout le monde, puis cliquez sur **suivant**. 
  
      ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp6.png)
  
  7. Dans l’écran configurer les autorisations de l’application, assurez-vous que **OpenID** et **user_impersonation** sont sélectionnés, puis cliquez sur **suivant**. 
  
      ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp7.png)
  
  8. Dans l’écran Résumé, cliquez sur **suivant**. 
  
  9. Dans l’écran terminé, cliquez sur **Fermer**.



## <a name="code-configuration"></a>Configuration du code 

Cette section montre comment configurer une application Web ASP.NET pour qu’elle se connecte et récupère un jeton pour appeler l’API Web. 

  1. Téléchargez l’exemple [ici](https://github.com/microsoft/adfs-sample-msal-dotnet-webapp-to-webapi)   
  
  2. Ouvrir l’exemple à l’aide de Visual Studio 
  
  3. Ouvrez le fichier Web. config. Modifiez les éléments suivants: 
       - Ida: ClientId: entrez la valeur de l' **identificateur du client** à partir de #3 dans inscription de l’application dans la section AD FS ci-dessus. 
       - Ida: ClientSecret-entrez la valeur **secrète** de #4 dans inscription de l’application dans la section AD FS ci-dessus. 
       - Ida: RedirectUri: entrez la valeur de l' **URI** de redirection de #3 dans inscription de l’application dans la section AD FS ci-dessus. 
       - Ida: Authority: entrez https://[votre AD FS nom d’hôte]/ADFS. Par exemple, https://adfs.contoso.com/adfs 
       - Ida: ressource: entrez la valeur de l' **identificateur** de #5 dans inscription de l’application dans la section AD FS ci-dessus. 
      
          ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp8.png)
 
 
### <a name="test-the-sample"></a>Tester l’exemple 
Cette section montre comment tester l’exemple configuré ci-dessus. 

  1. Une fois que le code a été modifié, régénérez la solution 
  
  2. En haut de Visual Studio, assurez-vous qu’Internet Explorer est sélectionné et cliquez sur la flèche verte. 
  
      ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp9.png)

  3. Sur la page d’hébergement, cliquez sur connexion. 
  
      ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp10.png)

  4. Vous serez redirigé vers la page de connexion AD FS. Continuez et connectez-vous. 
  
      ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp11.png)

  5. Une fois connecté, cliquez sur jeton d’accès.  
  
      ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp12.png)

  6. En cliquant sur le jeton d’accès, vous obtiendrez les informations du jeton d’accès en appelant l’API Web. 
  
      ![Ajouter un groupe d’applications](media/adfs-msal-web-app-web-api/webapp13.png)
 
 ## <a name="next-steps"></a>Étapes suivantes
[AD FS les flux OpenID Connect/OAuth et les scénarios d’application](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 