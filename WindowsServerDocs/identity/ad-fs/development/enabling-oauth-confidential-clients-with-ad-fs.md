---
ms.assetid: 5a64e790-6725-4099-aa08-8067d57c3168
title: Créer une application côté serveur à l’aide de clients de confidentialité OAuth avec AD FS 2016 ou version ultérieure
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8a8567a497e10df66f77fb996c937791b4aa9e08
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857562"
---
# <a name="build-a-server-side-application-using-oauth-confidential-clients-with-ad-fs-2016-or-later"></a>Créer une application côté serveur à l’aide de clients de confidentialité OAuth avec AD FS 2016 ou version ultérieure


Les versions 2016 et ultérieures prennent en charge les clients qui peuvent gérer leur propre secret, par exemple une application ou un service s’exécutant sur un serveur Web. AD FS  Ces clients sont appelés clients confidentiels.
Voici un schéma d’une application Web s’exécutant sur un serveur Web et servant de client confidentiel pour AD FS :  

## <a name="pre-requisites"></a>Prérequis  
La liste suivante répertorie les conditions préalables requises avant de terminer ce document. Ce document suppose que AD FS a été installé.  

-   Outils clients GitHub  

-   AD FS dans Windows Server 2016 TP4 ou version ultérieure  

-   Visual Studio 2013 ou version ultérieure.  

## <a name="create-an-application-group-in-ad-fs-2016-or-later"></a>Créer un groupe d’applications dans AD FS 2016 ou version ultérieure
La section suivante décrit comment configurer le groupe d’applications dans AD FS 2016 ou version ultérieure.  

#### <a name="create-the-application-group"></a>Créer le groupe d’applications  

1.  Dans AD FS gestion, cliquez avec le bouton droit sur groupes d’applications, puis sélectionnez **Ajouter un groupe d’applications**.  

2.  Dans l’Assistant groupe d’applications, pour le **nom** , entrez **ADFSOAUTHCC** , puis sous **applications client-serveur** , sélectionnez l' **application serveur qui accède à un modèle d’API Web** .  Cliquez sur **Suivant**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_2.PNG)  

3.  Copiez la valeur de l' **identificateur du client** .  Il sera utilisé ultérieurement comme valeur pour **Ida : ClientID** dans le fichier Web. config des applications.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_3.PNG)  

4.  Entrez les informations suivantes pour l' **URI de redirection :**  -  **https://localhost:44323** .  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.  

5.  Dans l’écran **configurer les informations d’identification** de l’application, activez la case à cocher **générer un secret partagé** et copier le secret.  Ce sera utilisé ultérieurement comme valeur pour **Ida : ClientSecret** dans le fichier Web. config des applications.  Cliquez sur **Suivant**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_4.PNG)   

6. Dans l’écran **configurer l’API Web** , entrez les informations suivantes pour **identificateur** -  **https://contoso.com/WebApp** .  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.  Cette valeur sera utilisée ultérieurement pour **Ida : GraphResourceId** dans le fichier Web. config des applications.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_9.PNG)  

7. Dans l’écran **appliquer la stratégie de Access Control** , sélectionnez **autoriser tout le monde** , puis cliquez sur **suivant**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  

8. Dans l’écran **configurer les autorisations** de l’application, assurez-vous que **OpenID** et **user_impersonation** sont sélectionnés, puis cliquez sur **suivant**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_8.PNG)  

9. Dans l’écran **Résumé** , cliquez sur **suivant**.  

10. Dans l’écran **terminé** , cliquez sur **Fermer**.  

## <a name="upgrade-the-database"></a>Mettre à niveau la base de données  
Visual Studio 2015 a été utilisé lors de la création de cette procédure pas à pas.   Pour faire en sorte que l’exemple fonctionne avec Visual Studio 2015, vous devez mettre à jour le fichier de base de données.  Utilisez la procédure suivante pour ce faire.  

Cette section explique comment télécharger l’exemple d’API Web et mettre à niveau la base de données dans Visual Studio 2015.   Nous allons utiliser l’exemple Azure AD qui se trouve [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity).  

Pour télécharger l’exemple de projet, utilisez git bash et tapez la commande suivante :  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity.git  
```  

![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_10.PNG)  

#### <a name="to-upgrade-the-database-file"></a>Pour mettre à niveau le fichier de base de données  

1.  Ouvrez le projet dans Visual Studio. une fenêtre contextuelle s’affiche pour vous indiquer que l’application requiert SQL Server 2012 Express, ou vous devrez mettre à niveau la base de données.  Cliquez sur OK.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_12.PNG)  

2.  Ensuite, compilez l’application en sélectionnant Build-> générer la solution en haut.  Cela permet de restaurer tous les packages NuGet.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_13.PNG)  

3.  Maintenant en haut, sélectionnez **afficher** -> **Explorateur de serveurs**.  Une fois que s’ouvre, sous **connexions de données**, cliquez avec le bouton droit sur **DefaultConnection** et sélectionnez **modifier la connexion**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_14.PNG)  

4.  Sur **modifier la connexion**, sous **nom du fichier de base de données (nouveau ou existant)** , sélectionnez **Parcourir** et indiquez **path\filename.mdf**. Cliquez sur **Oui** dans la boîte de dialogue.

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_6.PNG)

5.  Dans **modifier la connexion**, sélectionnez **avancé**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_15.PNG)  

6.  Dans les propriétés avancées, localisez la source de données et utilisez la liste déroulante pour la remplacer par ( **LocalDb\v11.0)** **\MSSQLLocalDB**.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_16.PNG)  

7.  Cliquez sur OK. Cliquez sur OK.  Cliquez sur Oui pour mettre à niveau la base de données.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_17.PNG)  

8.  Une fois cette étape terminée, sur la droite, copiez la valeur dans la zone en regard de **chaîne de connexion.**  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_18.PNG)  

9.  À présent, ouvrez le fichier Web. config et remplacez la valeur qui se trouve dans connectionString par la valeur que vous avez copiée ci-dessus.  Enregistrez le fichier Web.config.  

    > [!NOTE]  
    > Les étapes ci-dessus sont nécessaires pour que nous puissions récupérer la nouvelle connectionString.  Dans le cas contraire, lorsque nous exécutons la base de données Update-ci-dessous, une erreur se produira.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_19.PNG)  

10. En haut de Visual Studio, sélectionnez **afficher** -> autre **console du gestionnaire de package** **Windows** -> .  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_20.PNG)  

11. En bas, dans la console du gestionnaire de package, entrez : `Enable-Migrations` et appuyez sur entrée.  

    > [!NOTE]  
    > Si vous recevez une erreur indiquant que l’applet de commande Enable-migrations n’est pas reconnue en tant que cmdlet, entrez Install-Package EntityFramework pour mettre à jour l’EntityFramework.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_21.PNG)  

12. En bas, dans la console du gestionnaire de package, entrez : `Add-Migration <anynamehere>` et appuyez sur entrée.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_22.PNG)  

13. En bas, dans la console du gestionnaire de package, entrez : `Update-Database` et appuyez sur entrée.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_23.PNG)  

## <a name="modify-the-webapi-in-visual-studio"></a>Modifier WebApi dans Visual Studio  

#### <a name="to-modify-the-sample-web-api"></a>Pour modifier l’exemple d’API Web  

1.  Ouvrez l’exemple à l’aide de Visual Studio.  

2.  Ouvrez le fichier Web. config.  Modifiez les valeurs suivantes :  

    -   Ida : ClientId : entrez la valeur de #3 dans la section créer le groupe d’applications ci-dessus.  

    -   Ida : ClientSecret : entrez la valeur à partir de #5 dans la section créer le groupe d’applications ci-dessus.  

    -   Ida : GraphResourceId : entrez la valeur à partir de #6 dans la section créer le groupe d’applications ci-dessus.  

    ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_24.PNG)  

3.  Ouvrez le fichier Startup.Auth.cs sous App_Start et apportez les modifications suivantes :  

    -   Commentez les lignes suivantes :  

        ```  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   Ajoutez le code suivant dans la section :  

        ```  
        public static readonly string Authority = "https://<your_fsname>/adfs";  
        ```  

        où < your_fsname > est remplacé par la partie DNS de votre URL de service de Fédération, par exemple adfs.contoso.com  

        ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_25.PNG)  

4.  Ouvrez le fichier UserProfileController.cs et apportez les modifications suivantes :  

    -   Commentez ce qui suit :  

        ```  
        //authContext = new AuthenticationContext(Startup.Authority, new TokenDbCache(userObjectID));  
        ```  

    -   Remplacez les deux occurrences par les éléments suivants :  

        ```  
        authContext = new AuthenticationContext(Startup.Authority, false, new TokenDbCache(userObjectID));  
        ```  

        ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_27.PNG)  

    -   Commentez ce qui suit :  

        ```  
        //authContext = new AuthenticationContext(Startup.Authority);  
        ```  

    -   Remplacez les deux occurrences par les éléments suivants :  

        ```  
        authContext = new AuthenticationContext(Startup.Authority, false);  
        ```  

        ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_28.PNG)  

    -   Maintenant, commentez toutes les instances de ce qui suit :  

        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString() + "/OAuth");  
        ```  

    -   Remplacez toutes les occurrences par les éléments suivants :  

        ```  
        Uri redirectUri = new Uri(Request.Url.GetLeftPart(UriPartial.Authority).ToString());  
        ```  

        ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_34.PNG)  

## <a name="test-the-solution"></a>test de la solution  
Dans cette section, nous allons tester la solution cliente confidentielle.  Utilisez la procédure suivante pour tester la solution.  

#### <a name="testing-the-confidential-client-solution"></a>Test de la solution cliente confidentielle  

1. En haut de Visual Studio, assurez-vous qu’Internet Explorer est sélectionné et cliquez sur la flèche verte.  

   ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_36.png)  

2. Une fois la page ASP.Net remontée, cliquez sur **s’inscrire** en haut à droite de la page.  Entrez un nom d’utilisateur et un mot de passe, puis cliquez sur le bouton **Enregistrer** .  Cela crée un compte local dans la base de données SQL.  

   ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_31.PNG)  

3. Notez que le site ASP.NET indique Hello abby@contoso.com!.  Cliquez sur **Profile**.  

   ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_32.PNG)  

4. Cela ouvre une page sans informations et indique que vous devez cliquer ici pour vous connecter.  Cliquez **ici**.  

   ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_33.PNG)  

5. Vous êtes maintenant invité à vous connecter à AD FS.  

   ![AD FS OAuth](media/Enabling-Oauth-Confidential-Clients-with-AD-FS-2016/AD_FS_Confidential_35.PNG)  

## <a name="next-steps"></a>Étapes suivantes
[Développement des services AD FS](../../ad-fs/AD-FS-Development.md)  
