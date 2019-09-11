---
title: API Web d’AD FS MSAL appelant l’API Web (pour le compte du scénario)
description: Découvrez comment créer une API Web appelant une autre API Web.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2ab6141b84d03102c5dedd1ede0ba99e5adf3e4a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867747"
---
# <a name="scenario-web-api-calling-web-api-on-behalf-of-scenario"></a>Scénario : API Web appelant l’API Web (pour le compte du scénario) 
> S'applique à : AD FS 2019 et versions ultérieures 
 
Découvrez comment créer une API Web appelant une autre API Web pour le compte de l’utilisateur.  
 
Avant de lire cet article, vous devez connaître les [concepts de AD FS](../ad-fs-openid-connect-oauth-concepts.md) et le [Flow Behalf_Of](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#on-behalf-of-flow)

## <a name="overview"></a>Vue d'ensemble 


- Un client (application Web), qui n’est pas représenté dans le diagramme ci-dessous, appelle une API Web protégée et fournit un jeton de porteur JWT dans son en-tête http « Authorization ». 
- L’API Web protégée valide le jeton et utilise la méthode MSAL [AcquireTokenOnBehalfOf](https://docs.microsoft.com/en-us/dotnet/api/microsoft.identitymodel.clients.activedirectory.authenticationcontext.acquiretokenasync?view=azure-dotnet#Microsoft_IdentityModel_Clients_ActiveDirectory_AuthenticationContext_AcquireTokenAsync_System_String_Microsoft_IdentityModel_Clients_ActiveDirectory_ClientCredential_Microsoft_IdentityModel_Clients_ActiveDirectory_UserAssertion_) pour demander (à partir d’AD FS) un autre jeton afin qu’il puisse lui-même appeler une deuxième API Web (nommée l’API Web en aval) pour le compte de l’utilisateur. 
- L’API Web protégée utilise ce jeton pour appeler une API en aval. Il peut également appeler AcquireTokenSilentlater pour demander des jetons pour d’autres API en aval (mais toujours pour le compte du même utilisateur). AcquireTokenSilent actualise le jeton quand cela est nécessaire.  
 
     ![vue d’ensemble](media/adfs-msal-web-api-web-api/webapi1.png)
 
Pour mieux comprendre comment configurer pour le compte du scénario d’authentification dans ADFS, nous allons utiliser un exemple disponible [ici](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof) et suivre les étapes d’inscription et de configuration du code de l’application.  
 
## <a name="pre-requisites"></a>Prérequis 

- Outils clients GitHub 
- AD FS 2019 ou une version ultérieure configurée et en cours d’exécution 
- Visual Studio 2013 ou une version ultérieure 
 
## <a name="app-registration-in-ad-fs"></a>Inscription d’application dans AD FS 

Cette section montre comment inscrire l’application native en tant que client public et API Web en tant que parties de confiance (RP) dans AD FS 

  1. Dans AD FS gestion, cliquez avec le bouton droit sur **groupes d’applications** , puis sélectionnez Ajouter un **groupe d’applications**.  
  
  2. Dans l’Assistant groupe d’applications, pour le **nom** , entrez **WebApiToWebApi** , puis sous **applications client-serveur** , sélectionnez l' **application native qui accède à un modèle d’API Web** . Cliquez sur **Suivant**.

      ![Inscription de l’application](media/adfs-msal-web-api-web-api/webapi2.png)

  3. Copiez la valeur de l' **identificateur du client** . Il sera utilisé ultérieurement comme valeur pour **ClientID** dans le fichier **app. config** de l’application. Entrez les informations suivantes pour l' **URI de redirection :**  - . https://ToDoListClient Cliquez sur **Ajouter**. Cliquez sur **Suivant**. 
  
      ![Inscription de l’application](media/adfs-msal-web-api-web-api/webapi3.png)
  
  4. Dans l’écran configurer l’API Web, entrez l' **identificateur :** https://localhost:44321/. Cliquez sur **Ajouter**. Cliquez sur **Suivant**. Cette valeur sera utilisée ultérieurement dans les fichiers **app. config** et **Web. config** de l’application.  
 
      ![Inscription de l’application](media/adfs-msal-web-api-web-api/webapi4.png)

  5. Dans l’écran appliquer la stratégie de Access Control, sélectionnez **autoriser tout le monde** , puis cliquez sur **suivant**. 
  
      ![Inscription de l’application](media/adfs-msal-web-api-web-api/webapi5.png)  

  6. Dans l’écran configurer les autorisations de l’application, sélectionnez **OpenID** et **user_impersonation**. Cliquez sur **Suivant**.  
  
      ![Inscription de l’application](media/adfs-msal-web-api-web-api/webapi6.png)  

  7. Dans l’écran Résumé, cliquez sur **suivant**. 

  8. Dans l’écran terminé, cliquez sur **Fermer**. 


  9. Dans AD FS gestion, cliquez sur **groupes d’applications** , puis sélectionnez Groupe d’applications **WebApiToWebApi** . Effectuez un clic droit et sélectionnez **Propriétés**. 
  
      ![Inscription de l’application](media/adfs-msal-web-api-web-api/webapi7.png)  

  10. Dans l’écran Propriétés de WebApiToWebApi, cliquez sur **Ajouter une application...** . 
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi8.png)

  11. Sous applications autonomes, sélectionnez **application serveur**.  
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi9.png)

  12. Sur l’écran de l’application https://localhost:44321/ serveur, ajoutez en tant qu' **identificateur du client** et **URI de redirection**. 
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi10.png)

  13. Dans l’écran configurer les informations d’identification de l’application, sélectionnez **générer un secret partagé**. Copiez le secret pour une utilisation ultérieure.
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi11.png)

  14. Dans l’écran Résumé, cliquez sur **suivant**. 

  15. Dans l’écran terminé, cliquez sur **Fermer**. 

  16. Dans AD FS gestion, cliquez sur **groupes d’applications** , puis sélectionnez Groupe d’applications **WebApiToWebApi** . Effectuez un clic droit et sélectionnez **Propriétés**. 
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi12.png)

  17. Dans l’écran Propriétés de WebApiToWebApi, cliquez sur **Ajouter une application...** . 
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi13.png)

  18. Sous applications autonomes, sélectionnez **API Web**. 
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi14.png)  

  19. Dans configurer l’API Web, https://localhost:44300 ajoutez en tant qu' **identificateur**.  
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi15.png)

  20. Dans l’écran appliquer la stratégie de Access Control, sélectionnez **autoriser tout le monde** , puis cliquez sur **suivant**. 
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi16.png)

  21. Dans l’écran configurer les autorisations de l’application, cliquez sur **suivant**. 
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi17.png)

  22. Dans l’écran Résumé, cliquez sur **suivant**.

  23. Dans l’écran terminé, cliquez sur **Fermer**.  

  24. Cliquez sur OK dans WebApiToWebApi – écran Propriétés de Web API 2  

  25. Dans l’écran Propriétés de WebApiToWebApi, sélectionnez **WebApiToWebApi – API Web** , puis cliquez sur **modifier...** .  
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi18.png)

  26. Dans l’écran WebApiToWebApi – propriétés de l’API Web, sélectionnez l’onglet **règles de transformation d’émission** , puis cliquez sur Ajouter une **règle...** . 
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi19.png)

  27. Dans l’Assistant Ajouter une règle de revendication de transformation, sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée dans la** liste déroulante, puis cliquez sur **suivant**. 
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi20.png)

  28. Entrez **PassAllClaims** dans **nom de la règle de revendication :** champ et **x : [] = > problème (revendication = x);** règle de revendication dans le champ règle personnalisée :, puis cliquez sur Terminer.  
   
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi21.png)

  29. Cliquez sur OK dans WebApiToWebApi – écran Propriétés de l’API Web

  30. Dans l’écran Propriétés de WebApiToWebApi, sélectionnez Sélectionner WebApiToWebApi – API Web 2, puis cliquez sur modifier...</br> 
  ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi22.png)

  31. Sur l’écran WebApiToWebApi – propriétés de l’API Web 2, sélectionnez l’onglet Règles de transformation d’émission, puis cliquez sur Ajouter une règle... 

  32. Dans l’Assistant Ajouter une règle de revendication de transformation, sélectionnez Envoyer des revendications à l’aide d' ![une règle personnalisée à partir de dopdown, puis cliquez sur l’application suivante reg](media/adfs-msal-web-api-web-api/webapi23.png)

  33. Entrez PassAllClaims dans nom de la règle de revendication : champ et **x : [] = > problème (revendication = x);** règle de revendication dans le champ **règle personnalisée :** , puis cliquez sur **Terminer**.  
   
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi24.png)

  34.  Cliquez sur OK sur l’écran Propriétés de l’API Web 2 WebApiToWebApi, puis sur l’écran Propriétés de WebApiToWebApi.  
 

## <a name="code-configuration"></a>Configuration du code 

Cette section montre comment configurer une API Web pour appeler une autre API Web. 

  1. Téléchargez l’exemple [ici](https://github.com/microsoft/adfs-sample-msal-dotnet-webapi-to-webapi-onbehalfof)  
  
  2. Ouvrir l’exemple à l’aide de Visual Studio 
  
  3. Ouvrez le fichier app. config. Modifiez les éléments suivants : 
       - Ida : Authority : entrez https://[votre AD FS nom d’hôte]/ADFS/
       - Ida : ClientId : entrez la valeur de #3 dans inscription de l’application dans la section AD FS ci-dessus. 
       - Ida : RedirectUri-entrez la valeur de #3 dans inscription de l’application dans la section AD FS ci-dessus. 
       - TODO : TodoListResourceId – entrez la valeur de l’identificateur de #4 dans inscription de l’application dans la section AD FS ci-dessus 
       - Ida : TODO : TodoListBaseAddress-entrez la valeur de l’identificateur de #4 dans inscription de l’application dans la section AD FS ci-dessus. 
      
            ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi25.png)

  4. Ouvrez le fichier Web. config sous ToDoListService. Modifiez les éléments suivants : 
       - Ida : audience : entrez la valeur de l’identificateur du client à partir de #12 dans inscription de l’application dans la section AD FS ci-dessus
       - Ida : ClientId : entrez la valeur de l’identificateur du client à partir de #12 dans inscription de l’application dans la section AD FS ci-dessus. 
       - Ida ClientSecret : entrez le secret partagé copié à partir de #13 dans inscription de l’application dans la section AD FS ci-dessus.
       - Ida : RedirectUri-entrez la valeur RedirectUri de #12 dans inscription de l’application dans la section AD FS ci-dessus. 
       - Ida AdfsMetadataEndpoint : entrez https://[votre AD FS nom d’hôte]/FederationMetadata/2007-06/FederationMetadata.Xml 
       - Ida : OBOWebAPIBase-entrez la valeur de l’identificateur de #19 dans inscription de l’application dans la section AD FS ci-dessus. 
       - Ida : Authority : entrez https://[votre AD FS nom d’hôte]/ADFS 
  
          ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi26.png) 

 5. Ouvrez le fichier Web. config sous WebAPIOBO. Modifiez les éléments suivants : 
       - Ida AdfsMetadataEndpoint : entrez https://[votre AD FS nom d’hôte]/FederationMetadata/2007-06/FederationMetadata.Xml 
       - Ida : audience : entrez la valeur de l’identificateur du client à partir de #12 dans inscription de l’application dans la section AD FS ci-dessus 
 
          ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi27.png)
 
## <a name="test-the-sample"></a>Tester l’exemple 

Cette section montre comment tester l’exemple configuré ci-dessus. 

Une fois que le code a été modifié, régénérez la solution 
 
  1. Dans Visual Studio, cliquez avec le bouton droit sur solution, puis sélectionnez **définir les projets de démarrage...** 
      
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi28.png)

  2. Dans les pages propriétés, vérifiez que **action** est défini sur **Démarrer** pour chacun des projets, à l’exception de TodoListSPA.  
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi29.png)
  
  3. En haut de Visual Studio, cliquez sur la flèche verte.  
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi30.png)

  4. Dans l’écran principal de l’application native, cliquez sur **connexion**. 
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi31.png)

     Si vous ne voyez pas l’écran de l’application native, recherchez et supprimez les fichiers * msalcache. bin du dossier où le projet référentiel est enregistré sur votre système. 
  
  5. Vous serez redirigé vers la page de connexion AD FS. Continuez et connectez-vous. 
  
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi32.png)

  6. Une fois connecté, entrez l’API Web de texte vers l’appel d’API Web dans la **créer un élément à faire**. Cliquez sur **Ajouter un élément**.  Cela appellera l’API Web (service de liste de tâches) qui appelle ensuite l’API Web 2 (WebAPIOBO) et ajoute l’élément dans le cache.  
 
      ![Reg de l’application](media/adfs-msal-web-api-web-api/webapi33.png)
 
 ## <a name="next-steps"></a>Étapes suivantes
[Flux OpenID Connect/OAuth avec AD FS et scénarios d’application](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 
 
