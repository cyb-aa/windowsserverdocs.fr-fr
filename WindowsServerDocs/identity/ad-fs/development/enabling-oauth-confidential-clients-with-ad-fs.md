---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: "Créer une application du côté serveur à l’aide de clients confidentiels OAuth avec ADFS2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 175c683f9097aeba4c1f06e8671183476c98aa3f
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016"></a>Créer une application du côté serveur à l’aide de clients confidentiels OAuth avec ADFS2016

>S’applique à: Windows Server2016

S’appuyant sur la prise en charge Oauth initiale dans ADFS dans Windows Server2012R2, ADFS2016 introduit la prise en charge pour les clients capables de maintenir leur propre secret, par exemple, une application ou un service en cours d’exécution sur un serveur web.  Ces clients sont connus en tant que clients confidentielles.    
Voici un schéma d’une application web en cours d’exécution sur un serveur web et agissant comme un client confidentiel à ADFS:  
  
## <a name="pre-requisites"></a>Conditions préalables  
Voici une liste des conditions préalables requises avant la fin de ce document. Ce document suppose que ADFS a été installé et une batterie ADFS a été créée.  
  
-   Un abonnement Azure AD (une version d’évaluation gratuite est parfaite)  
  
-   Outils du client GitHub  
  
-   ADFS dans Windows Server2016 TP4 ou version ultérieure  
  
-   Visual Studio2013 ou version ultérieure.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Créer un groupe d’applications dans ADFS2016  
La section suivante décrit comment configurer le groupe d’applications dans ADFS2016.  
  
#### <a name="create-the-application-group"></a>Créer le groupe d’applications  
  
1.  Dans Gestion ADFS, avec le bouton droit sur les groupes d’applications et sélectionnez **ajouter un groupe d’Application**.  
  
2.  Dans l’Assistant groupe d’Application, pour le nom, entrez **ADFSOAUTHCC** sous **applications autonomes** sélectionner le **application serveur ou le site Web** modèle.  Cliquez sur **suivant**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  
  
3.  Copie le **identificateur de Client** valeur.  Il sera utilisé plus tard en tant que la valeur de **ida: ClientId** dans le fichier web.config d’application.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  
  
4.  Entrez les informations suivantes pour **URI de redirection:** - **https://localhost:44323**.  Cliquez sur **ajouter**. Cliquez sur **suivant**.  
  
5.  Sur le **configurer les informations d’identification de l’Application** écran, placez la case à cocher **générer un secret partagé**et copiez la clé secrète.  Il sera utilisé plus tard en tant que la valeur de **ida: AppKey** dans le fichier web.config d’application.  Cliquez sur **suivant**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)  
  
6.  Sur le **Résumé**, cliquez sur **suivant**.  
  
7.  Sur le **Complete**, cliquez sur **fermer**.  
  
8.  Maintenant, sur le nouveau groupe d’Application avec le bouton droit et sélectionnez **propriétés**.  
  
9. Sur **ADFSOAUTHCC propriétés** cliquez sur **ajouter application**.  
  
10. Sur le **ajouter une nouvelle application à l’exemple d’Application** sélectionnez **API Web**et cliquez sur **suivant**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)  
  
11. Sur le **configurer des API Web** de l’écran, entrez les informations suivantes pour **identificateur** - **https://contoso.com/WebApp**.  Cliquez sur **ajouter**. Cliquez sur **suivant**.  Cette valeur sera utilisée ultérieurement pour **ida: GraphResourceId** dans le fichier web.config d’application.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  
  
12. Sur le **stratégie de contrôle d’accès choisir** écran, puis sélectionnez **autoriser tout le monde** et cliquez sur **suivant**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. Sur le **configurer des autorisations Application** écran, assurez-vous que**user_impersonation** est sélectionné, cliquez sur **suivant**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  
  
14. Sur le **Résumé**, cliquez sur **suivant**.  
  
15. Sur le **Complete**, cliquez sur **fermer**.  
  
16. Sur le **ADFSOAUTHCC propriétés** cliquez sur **OK**.  
  
## <a name="upgrade-the-database"></a>Mise à niveau la base de données  
Visual Studio2015 a été utilisé lors de la création de cette procédure pas à pas.   Pour obtenir de l’exemple d’utilisation de Visual Studio2015, vous devrez mettre à jour le fichier de base de données.  Pour ce faire, utilisez la procédure suivante.  
  
Cette section explique comment télécharger l’exemple API Web et de mettre à niveau la base de données dans Visual Studio2015.   Nous allons utiliser l’exemple d’Azure AD qui est [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity).  
  
Pour télécharger l’exemple de projet, utiliser Git Bash et tapez la commande suivante:  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  
  
![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  
  
#### <a name="to-upgrade-the-database-file"></a>Pour mettre à niveau le fichier de base de données  
  
1.  Ouvrez le projet dans Visual Studio, il y aura une fenêtre contextuelle indiquant que l’application requiert Express de SQLServer2102 ou vous devez mettre à niveau la base de données.  Cliquez sur Ok.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  
  
2.  L’application en sélectionnant la génération de compilation suivante -> générer la Solution en haut.  Cette opération restaure tous les packages NuGet.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  
  
3.  Désormais, dans la partie supérieure, sélectionnez **affichage** -> **Server Explorer**.  Une fois qui s’ouvre, sous **connexions de données**, avec le bouton droit **DefaultConnection** et sélectionnez **modifier la connexion**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  
  
4.  Sur **modifier la connexion**, sélectionnez **avancé**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  
  
5.  Sur les propriétés avancées, recherchez la Source de données et utilisez le menu déroulant pour le modifier à partir de **(LocalDb\v11.0)** à **MSSQLLocalDB (LoaclDB)**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  
  
6.  Cliquez sur Ok. Cliquez sur Ok.  Cliquez sur Oui pour mettre à niveau la base de données.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  
  
7.  Après cela, plus à droite, copiez la valeur dans la zone regard **chaîne de connexion.**  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  
  
8.  Maintenant, ouvrez le fichier Web.config et remplacez la valeur qui se trouve dans connectionString avec la valeur que vous avez copié ci-dessus.  Enregistrez le fichier Web.config.  
  
    > [!NOTE]  
    > Les étapes ci-dessus sont nécessaires afin que nous puissions la nouvelle chaîne de connexion.  Dans le cas contraire, lorsque nous exécutons Update-Database ci-dessous, il sera d’erreur.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  
  
9. Dans la partie supérieure de Visual Studio, sélectionnez **affichage** -> **autres fenêtres** -> **Console du Gestionnaire de Package**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  
  
10. En bas, dans la Console du Gestionnaire de Package, entrez: `Enable-Migrations`et appuyez sur entrer.  
  
    > [!NOTE]  
    > Si vous obtenez une erreur indiquant qu’Enable-Migrations n’est pas reconnu comme une applet de commande, entrez EntityFramework Install-Package pour mettre à jour le EntityFramework.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  
  
11. En bas, dans la Console du Gestionnaire de Package, entrez: `Add-Migration <anynamehere>`et appuyez sur entrer.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  
  
12. En bas, dans la Console du Gestionnaire de Package, entrez: `Update-Database`et appuyez sur entrer.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  
  
## <a name="modify-the-webapi-in-visual-studio"></a>Modifier le WebApi dans Visual Studio  
  
#### <a name="to-modify-the-sample-web-api"></a>Pour modifier l’API Web d’exemple  
  
1.  Ouvrez l’exemple à l’aide de Visual Studio.  
  
2.  Ouvrez le fichier web.config.  Modifiez les valeurs suivantes:  
  
    -   IDA: ClientId - Entrez la valeur de 3 # ci-dessus.  
  
    -   IDA: AppKey - Entrez la valeur de 5 # ci-dessus.  
  
    -   IDA: GraphResourceId - Entrez la valeur #11 ci-dessus.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  
  
3.  Ouvrez le fichier Startup.Auth.cs sous App_Start et apportez les modifications suivantes:  
  
    -   Commentez les lignes suivantes:  
  
        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
        ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  
  
    -   Ajoutez le code suivant place:  
  
        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  
  
        Où < your_fsname > est remplacé par la partie DNS de votre url de service de fédération, par exemple adfs.contoso.com  
  
        ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_26.PNG)  
  
4.  Ouvrez le fichier UserProfileController.cs et apportez les modifications suivantes:  
  
    -   Commentez les éléments suivants:  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  
  
    -   Remplacez les deux occurrences avec les éléments suivants:  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  
  
        ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  
  
    -   Commentez les éléments suivants:  
  
        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  
  
    -   Remplacez les deux occurrences avec les éléments suivants:  
  
        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  
  
        ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  
  
    -   Désormais commentez toutes les instances des opérations suivantes:  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString() + "/OAuth");  
        ```  
  
    -   Remplacer toutes les occurrences avec les éléments suivants:  
  
        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority.ToString());  
        ```  
  
        ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  
  
## <a name="test-the-solution"></a>Tester la Solution  
Dans cette section, nous testerez la solution client confidentielles.  Utilisez la procédure suivante pour tester la solution.  
  
#### <a name="testing-the-confidential-client-solution"></a>Test de la solution client confidentielles  
  
1.  Dans la partie supérieure de Visual Studio, assurez-vous que Internet Explorer est activée, puis cliquez sur la flèche verte.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  
  
2.  Une fois que la page ASP.Net s’affiche, cliquez sur **inscrire**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
3.  Entrez un nom d’utilisateur et le mot de passe, puis **inscrire**.  Cela crée un compte local dans la base de données SQL.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  
  
4.  Avis, le site ASP.NET indique à présent bsimon Hello!.  Cliquez sur **profil**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  
  
5.  Cela affiche une page sans toutes les informations et indique que nous devons cliquez ici pour vous connecter.  Cliquez sur **ici**.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  
  
6.  Vous demandera maintenant de connexion à ADFS.  
  
    ![ADFS Oauth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  
  
## <a name="next-steps"></a>Étapes suivantes
[Développement d’ADFS](../../ad-fs/AD-FS-Development.md)  

