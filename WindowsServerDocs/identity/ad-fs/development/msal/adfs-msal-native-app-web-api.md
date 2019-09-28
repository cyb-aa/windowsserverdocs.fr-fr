---
title: AD FS application native MSAL appelant l’API Web
description: Découvrez comment créer des utilisateurs de connexion d’application natives authentifiés par AD FS 2019 et acquérant des jetons à l’aide de la bibliothèque MSAL pour appeler des API Web.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ff76a6dffd66296a02cffcbd79bc6dfadc91c14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407793"
---
# <a name="scenario-native-app-calling-web-api"></a>Scénario : Application native appelant l’API Web 
>S'applique à : AD FS 2019 et versions ultérieures 
 
Découvrez comment créer des utilisateurs de connexion d’applications natives authentifiés par AD FS 2019 et acquérant des jetons à l’aide de la [bibliothèque MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki) pour appeler des API Web.  
 
Avant de lire cet article, vous devez vous familiariser avec les [concepts de AD FS](../ad-fs-openid-connect-oauth-concepts.md) et le [flow d’octroi de code d’autorisation](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md#authorization-code-grant-flow)
 
## <a name="overview"></a>Vue d'ensemble 
 
 ![Vue d'ensemble](media/adfs-msal-native-app-web-api/native1.png)

Dans ce processus, vous ajoutez l’authentification à votre application native (client public), qui peut donc connecter des utilisateurs et appeler une API Web. Pour appeler une API Web à partir d’une application native qui connecte des utilisateurs, vous pouvez utiliser la méthode d’acquisition de jetons [AcquireTokenInteractive](https://docs.microsoft.com/en-us/dotnet/api/microsoft.identity.client.ipublicclientapplication.acquiretokeninteractive?view=azure-dotnet#Microsoft_Identity_Client_IPublicClientApplication_AcquireTokenInteractive_System_Collections_Generic_IEnumerable_System_String__) de MSAL. Pour activer cette interaction, MSAL s’appuie sur un navigateur Web. 

 
Pour mieux comprendre comment configurer une application native dans ADFS pour obtenir un jeton d’accès de manière interactive, nous allons utiliser un exemple disponible [ici](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi) et suivre les étapes d’inscription et de configuration du code de l’application.  
 

## <a name="pre-requisites"></a>Prérequis 


- Outils clients GitHub 
- AD FS 2019 ou une version ultérieure configurée et en cours d’exécution 
- Visual Studio 2013 ou version ultérieure 
 

## <a name="app-registration-in-ad-fs"></a>Inscription d’application dans AD FS 
Cette section montre comment inscrire l’application native comme un client public et une API Web en tant que partie de confiance (RP) dans AD FS 

  1. Dans **AD FS gestion**, cliquez avec le bouton droit sur **groupes d’applications** , puis sélectionnez Ajouter un **groupe d’applications**.   
  
  2. Dans l’Assistant groupe d’applications, pour le **nom** , entrez **NativeAppToWebApi** , puis sous **applications client-serveur** , sélectionnez l' **application native qui accède à un modèle d’API Web** . Cliquez sur **Suivant**.  
  
      ![Reg de l’application](media/adfs-msal-native-app-web-api/native2.png)  

  3. Copiez la valeur de l' **identificateur du client** . Il sera utilisé ultérieurement comme valeur pour **ClientID** dans le fichier **app. config** de l’application. Entrez les informations suivantes pour l' **URI de redirection :** https://ToDoListClient. Cliquez sur **Ajouter**. Cliquez sur **Suivant**.  
 
     ![Reg de l’application](media/adfs-msal-native-app-web-api/native3.png) 

  4. Dans l’écran configurer l’API Web, entrez l' **identificateur :** https://localhost:44321/. Cliquez sur **Ajouter**. Cliquez sur **Suivant**. Cette valeur sera utilisée ultérieurement dans les fichiers **app. config** et **Web. config** de l’application.
 
     ![Reg de l’application](media/adfs-msal-native-app-web-api/native4.png)   
  
  5. Dans l’écran appliquer la stratégie de Access Control, sélectionnez **autoriser tout le monde** , puis cliquez sur **suivant**. 
  
     ![Reg de l’application](media/adfs-msal-native-app-web-api/native5.png)   
  
  6. Dans l’écran configurer les autorisations de l’application, assurez-vous que **OpenID** est sélectionné, puis cliquez sur **suivant**.  
     
     ![Reg de l’application](media/adfs-msal-native-app-web-api/native6.png) 

  7. Dans l’écran Résumé, cliquez sur **suivant**.
  
  8. Dans l’écran terminé, cliquez sur **Fermer**. 
  
  9. Dans AD FS gestion, cliquez sur **groupes d’applications** , puis sélectionnez Groupe d’applications **NativeAppToWebApi** . Effectuez un clic droit et sélectionnez **Propriétés**.
  
      ![Reg de l’application](media/adfs-msal-native-app-web-api/native7.png)

  10. Dans l’écran Propriétés de NativeAppToWebApi, sélectionnez **NativeAppToWebApi – API Web** sous **API Web** , puis cliquez sur **modifier...** 
  
      ![Reg de l’application](media/adfs-msal-native-app-web-api/native8.png) 

  11. Dans l’écran NativeAppToWebApi – propriétés de l’API Web, sélectionnez l’onglet **règles de transformation d’émission** , puis cliquez sur Ajouter une **règle...** 
  
      ![Reg de l’application](media/adfs-msal-native-app-web-api/native9.png) 

  12. Dans l’Assistant Ajouter une règle de revendication de transformation, sélectionnez **transformer une revendication entrante** dans la liste déroulante **modèle de règle de revendication** , puis cliquez sur **suivant**.  
  
      ![Reg de l’application](media/adfs-msal-native-app-web-api/native10.png) 

  13. Entrez **NameID** dans le champ nom de la **règle de revendication :** . Sélectionnez le **nom** du **type de revendication entrante :** , **ID de nom** pour le type de **revendication sortante :** et **nom commun** pour le **format d’ID de nom sortant :** . Cliquez sur **Terminer**.
  
      ![Reg de l’application](media/adfs-msal-native-app-web-api/native11.png) 

  14. Cliquez sur OK dans l’écran Propriétés de l’API Web NativeAppToWebApi, puis sur écran Propriétés de NativeAppToWebApi.  
 
## <a name="code-configuration"></a>Configuration du code 
Cette section montre comment configurer une application native pour qu’elle se connecte et récupère un jeton pour appeler l’API Web. 

1. Téléchargez l’exemple [ici](https://github.com/microsoft/adfs-sample-msal-dotnet-native-to-webapi) 

2. Ouvrir l’exemple à l’aide de Visual Studio 

3. Ouvrez le fichier app. config. Modifiez les éléments suivants : 
   - Ida : Authority : entrez https://[votre AD FS nom d’hôte]/ADFS
   - Ida : ClientId : entrez la valeur de l' **identificateur du client** à partir de #3 dans inscription de l’application dans la section AD FS ci-dessus. 
   - Ida : RedirectUri : entrez la valeur de l' **URI de redirection** de #3 dans inscription de l’application dans la section AD FS ci-dessus.
   - TODO : TodoListResourceId – entrez la valeur de l' **identificateur** de #4 dans inscription de l’application dans la section AD FS ci-dessus 
   - Ida : TODO : TodoListBaseAddress-entrez la valeur de l' **identificateur** de #4 dans inscription de l’application dans la section AD FS ci-dessus. 
 
     ![configuration du code](media/adfs-msal-native-app-web-api/native12.png)

 4. Ouvrez le fichier Web. config. Modifiez les éléments suivants : 
    - Ida : audience : entrez la valeur de l' **identificateur** de #4 dans inscription de l’application dans la section AD FS ci-dessus 
    - Ida AdfsMetadataEndpoint : entrez https://[votre AD FS nom d’hôte]/FederationMetadata/2007-06/FederationMetadata.Xml 
    
      ![configuration du code](media/adfs-msal-native-app-web-api/native13.png)
 
  
## <a name="test-the-sample"></a>Tester l’exemple 
Cette section montre comment tester l’exemple configuré ci-dessus. 

  1. Une fois que le code a été modifié, régénérez la solution 
 
  2. Dans Visual Studio, cliquez avec le bouton droit sur solution, puis sélectionnez **définir les projets de démarrage...**  
 
     ![Test de l’application](media/adfs-msal-native-app-web-api/native14.png)

  3. Dans les pages propriétés, vérifiez que **action** est défini sur **Démarrer** pour chacun des projets 
      
     ![Test de l’application](media/adfs-msal-native-app-web-api/native15.png)

  4. En haut de Visual Studio, cliquez sur la flèche verte.  
 
     ![Test de l’application](media/adfs-msal-native-app-web-api/native16.png)

  5. Dans l’écran principal de l’application native, cliquez sur **connexion**.  
  
     ![Test de l’application](media/adfs-msal-native-app-web-api/native17.png)

    Si vous ne voyez pas l’écran de l’application native, recherchez et supprimez les fichiers * msalcache. bin du dossier où le projet référentiel est enregistré sur votre système. 

  6. Vous serez redirigé vers la page de connexion AD FS. Continuez et connectez-vous. 
  
      ![Test de l’application](media/adfs-msal-native-app-web-api/native18.png)

  7. Une fois connecté, entrez texte **générer une application native pour l’API Web** dans la **créer un élément à faire**. Cliquez sur **Ajouter un élément**.  Cela appellera le **service de liste des tâches (API Web)** et ajoutera l’élément dans le cache. 
    
       ![Test de l’application](media/adfs-msal-native-app-web-api/native19.png)
 
## <a name="next-steps"></a>Étapes suivantes
[Flux OpenID Connect/OAuth avec AD FS et scénarios d’application](../../overview/ad-fs-openid-connect-oauth-flows-scenarios.md)
 