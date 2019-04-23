---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: Créer une application du côté serveur à l’aide de clients confidentiels OAuth avec AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 175c683f9097aeba4c1f06e8671183476c98aa3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869580"
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016"></a>Créer une application du côté serveur à l’aide de clients confidentiels OAuth avec AD FS 2016

>S'applique à : Windows Server 2016

S’appuyant sur la prise en charge Oauth initiale dans AD FS dans Windows Server 2012 R2, AD FS 2016 introduit la prise en charge pour les clients capables de préserver leur propre secret, telle qu’une application ou un service en cours d’exécution sur un serveur web.  Ces clients sont connus comme les clients confidentiels.    
Voici un schéma d’une application web en cours d’exécution sur un serveur web et en agissant comme un client confidentiel à AD FS :  
  
## <a name="pre-requisites"></a>Conditions préalables  
Voici une liste de conditions préalables qui sont nécessaires avant la fin de ce document. Ce document suppose que AD FS a été installé et une batterie de serveurs AD FS a été créé.  
  
-   Un abonnement Azure AD (un essai gratuit est normale)  
  
-   Outils clients de GitHub  
  
-   AD FS dans Windows Server 2016 TP4 ou version ultérieure  
  
-   Visual Studio 2013 ou version ultérieure.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Créer un groupe d’applications dans AD FS 2016  
La section suivante décrit comment configurer le groupe d’applications dans AD FS 2016.  
  
#### <a name="create-the-application-group"></a>Créer le groupe d’applications  
  
1.  Dans Gestion AD FS, avec le bouton droit sur les groupes de l’Application et sélectionnez **ajouter l’Application groupe**.  
  
2.  Dans l’Assistant de groupe d’Application, pour le nom, entrez **ADFSOAUTHCC** et sous **applications autonomes** sélectionner le **application serveur ou un site Web** modèle.  Cliquez sur **Suivant**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  
  
3.  Copie le **identificateur Client** valeur.  Il sera utilisé ultérieurement comme valeur pour **ida : ClientId** dans le fichier web.config des applications.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  
  
4.  Entrez les informations suivantes **URI de redirection :** - **https://localhost:44323**.  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.  
  
5.  Sur le **configurer les informations d’identification de l’Application** écran, cochez la case **générer un secret partagé**et copiez la clé secrète.  Il sera utilisé ultérieurement comme valeur pour **ida : AppKey** dans le fichier web.config des applications.  Cliquez sur **Suivant**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)  
  
6.  Sur le **Résumé** , cliquez sur **suivant**.  
  
7.  Sur le **Terminer** , cliquez sur **fermer**.  
  
8.  Maintenant, sur le nouveau groupe d’applications avec le bouton droit et sélectionnez **propriétés**.  
  
9. Sur **ADFSOAUTHCC propriétés** cliquez sur **ajouter application**.  
  
10. Sur le **ajouter une nouvelle application à l’exemple d’Application** sélectionnez **API Web**et cliquez sur **suivant**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)  
  
11. Sur le **configurer des API Web** écran, entrez les informations suivantes **identificateur** - **https://contoso.com/WebApp**.  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.  Cette valeur sera utilisée ultérieurement pour **ida : GraphResourceId** dans le fichier web.config des applications.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  
  
12. Sur le **choisir la stratégie de contrôle d’accès** s’affiche, sélectionnez **autoriser tout le monde** et cliquez sur **suivant**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. Sur le **configurer les autorisations Application** écran, vérifiez que**user_impersonation** est sélectionné, cliquez sur **suivant**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  
  
14. Sur le **Résumé** , cliquez sur **suivant**.  
  
15. Sur le **Terminer** , cliquez sur **fermer**.  
  
16. Sur le **ADFSOAUTHCC propriétés** cliquez sur **OK**.  
  
## <a name="upgrade-the-database"></a>Mise à niveau la base de données  
Visual Studio 2015 a été utilisé lors de la création de cette procédure pas à pas.   Pour obtenir l’exemple d’utilisation de Visual Studio 2015, vous devrez mettre à jour le fichier de base de données.  Utilisez la procédure suivante pour ce faire.  
  
Cette section explique comment télécharger l’exemple d’API Web et de mise à niveau de la base de données dans Visual Studio 2015.   Nous allons utiliser l’exemple d’Azure AD est [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity).  
  
Pour télécharger l’exemple de projet, utilisez Git Bash et tapez la commande suivante :  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  
  
![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  
  
#### <a name="to-upgrade-the-database-file"></a>Pour mettre à niveau le fichier de base de données  
  
1.  Ouvrez le projet dans Visual Studio, il y aura un message vous indiquant que l’application requiert SQL Server 2102 Express, ou vous devez mettre à niveau la base de données.  Cliquez sur Ok.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  
  
2.  Prochaine compilation de l’application en sélectionnant Générer -> Générer la Solution en haut.  Cette opération restaure tous les packages NuGet.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  
  
3.  Maintenant, en haut, sélectionnez **vue** -> **Explorateur de serveurs**.  Une fois qui s’ouvre, sous **des connexions de données**, avec le bouton droit **DefaultConnection** et sélectionnez **modifier la connexion**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  
  
4.  Sur **modifier la connexion**, sélectionnez **avancé**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  
  
5.  Sur les propriétés avancées, recherchez la Source de données et permet de faire passer de la liste déroulante **(LocalDB \ V11.0)** à **(LoaclDB) MSSQLLocalDB**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  
  
6.  Cliquez sur Ok. Cliquez sur Ok.  Cliquez sur Oui pour mettre à niveau la base de données.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  
  
7.  Fin de cette procédure, plus à droite, copiez la valeur dans la zone à côté **chaîne de connexion.**  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  
  
8.  À présent, ouvrez le fichier Web.config et remplacez la valeur qui se trouve dans connectionString avec la valeur que vous avez copié ci-dessus.  Enregistrez le fichier Web.config.  
  
    > [!NOTE]  
    > Les étapes ci-dessus sont nécessaires afin que nous puissions obtenir la nouvelle chaîne de connexion.  Sinon, lorsque nous exécutons la mise à jour la base de données ci-dessous une erreur est générée.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  
  
9. En haut de Visual Studio, sélectionnez **vue** -> **Windows autres** -> **Console du Gestionnaire de Package**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  
  
10. En bas, dans la Console du Gestionnaire de Package, entrez : `Enable-Migrations` puis appuyez sur ENTRÉE.  
  
    > [!NOTE]  
    > Si vous obtenez une erreur indiquant que Enable-Migrations n’est pas reconnue comme une applet de commande, entrez Install-Package EntityFramework pour mettre à jour de l’Entity Framework.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  
  
11. En bas, dans la Console du Gestionnaire de Package, entrez : `Add-Migration <anynamehere>` puis appuyez sur ENTRÉE.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  
  
12. En bas, dans la Console du Gestionnaire de Package, entrez : `Update-Database` puis appuyez sur ENTRÉE.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  
  
## <a name="modify-the-webapi-in-visual-studio"></a>Modifier l’API Web dans Visual Studio  
  
#### <a name="to-modify-the-sample-web-api"></a>Pour modifier l’exemple d’API Web  
  
1.  Ouvrez l’exemple à l’aide de Visual Studio.  
  
2.  Ouvrez le fichier web.config.  Modifiez les valeurs suivantes :  
  
    -   IDA : ClientId - Entrez la valeur à partir de #3 ci-dessus.  
  
    -   IDA : AppKey - Entrez la valeur à partir de #5 ci-dessus.  
  
    -   ida:GraphResourceId - enter the value from #11 above.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  
  
3.  Ouvrez le fichier Startup.Auth.cs sous App_Start et apportez les modifications suivantes :  
  
    -   Commentez les lignes suivantes :  
  
        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  
  
    -   Ajoutez le code suivant place :  
  
        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  
  
        où < your_fsname > est remplacé par la partie DNS de votre url de service de fédération, par exemple adfs.contoso.com  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_26.PNG)  
  
4.  Ouvrez le fichier UserProfileController.cs et apportez les modifications suivantes :  
  
    -   Commentez les éléments suivants :  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  
  
    -   Remplacez les deux occurrences avec les éléments suivants :  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  
  
    -   Commentez les éléments suivants :  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  
  
    -   Remplacez les deux occurrences avec les éléments suivants :  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  
  
    -   Maintenant commentez toutes les instances de ce qui suit :  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString() + "/OAuth");  
        ```  
  
    -   Remplacez toutes les occurrences avec les éléments suivants :  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString());  
        ```  
  
        ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  
  
## <a name="test-the-solution"></a>Tester la Solution  
Dans cette section, nous allons tester la solution de client confidentiel.  Utilisez la procédure suivante pour tester la solution.  
  
#### <a name="testing-the-confidential-client-solution"></a>Test de la solution de client confidentiel  
  
1.  En haut de Visual Studio, assurez-vous que Internet Explorer est activée, puis cliquez sur la flèche verte.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  
  
2.  Une fois la page ASP.Net s’affiche, cliquez sur **inscrire**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
3.  Entrez un nom d’utilisateur et le mot de passe, puis activez **inscrire**.  Cela crée un compte local dans la base de données SQL.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
4.  Notez que, le site ASP.NET indique à présent Hello bsimon !.  Cliquez sur **profil**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  
  
5.  Cela permet d’afficher une page sans les informations et indique que nous devons cliquez ici pour vous connecter.  Cliquez sur **ici**.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  
  
6.  Vous devez maintenant se connecter à AD FS.  
  
    ![AD FS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  
  
## <a name="next-steps"></a>Étapes suivantes
[Développement de AD FS](../../ad-fs/AD-FS-Development.md)  

