---
title: Créer une application cliente native à l’aide de clients publics OAuth avec AD FS 2016 ou version ultérieure
description: Procédure pas à pas qui fournit des instructions pour la génération d’une application cliente native à l’aide de clients publics OAuth et AD FS 2016 ou version ultérieure
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.openlocfilehash: 96659164a9eea1784cb529c47dd58be70d546f80
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948732"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016-or-later"></a>Créer une application cliente native à l’aide de clients publics OAuth avec AD FS 2016 ou version ultérieure

## <a name="overview"></a>Vue d'ensemble

Cet article explique comment créer une application native qui interagit avec une API Web protégée par AD FS 2016 ou version ultérieure.

1. L’application .net TodoListClient WPF utilise le Bibliothèque d’authentification Active Directory (ADAL) pour obtenir un jeton d’accès JWT à partir d’Azure Active Directory (Azure AD) par le biais du protocole OAuth 2,0
2. Le jeton d’accès est utilisé comme un jeton de porteur pour authentifier l’utilisateur lors de l’appel du point de terminaison/ToDoList de l’API Web TodoListService.
 Nous allons utiliser l’exemple d’application pour Azure AD ici, puis le modifier pour AD FS 2016 ou une version ultérieure.

![Présentation de l’application](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>Prérequis
La liste suivante répertorie les conditions préalables requises avant de terminer ce document. Ce document suppose que AD FS a été installé et qu’une batterie de AD FS a été créée.

* Outils clients GitHub
* AD FS dans Windows Server 2016 ou version ultérieure
* Visual Studio 2013 ou une version ultérieure

## <a name="creating-the-sample-walkthrough"></a>Création de l’exemple de procédure pas à pas

### <a name="create-the-application-group-in-ad-fs"></a>Créez le groupe d’applications dans AD FS

1. Dans AD FS gestion, cliquez avec le bouton droit sur **groupes d’applications** , puis sélectionnez Ajouter un **groupe d’applications**.

2. Dans l’Assistant groupe d’applications, pour le nom, entrez le nom de votre choix, par exemple NativeToDoListAppGroup. Sélectionnez l' **application native qui accède à un modèle d’API Web** . Cliquez sur **Suivant**.
 ![ajouter un groupe d’applications](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. Dans la page **application native** , notez l’identificateur généré par AD FS. Il s’agit de l’ID avec lequel AD FS reconnaît l’application cliente publique. Copiez la valeur de l' **identificateur du client** . Il sera utilisé ultérieurement comme valeur pour **Ida : ClientID** dans le code de l’application. Si vous le souhaitez, vous pouvez fournir n’importe quel identificateur personnalisé ici. L’URI de redirection est une valeur arbitraire, par exemple, put https://ToDoListClient ![ application native](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. Dans la page **configurer l’API Web** , définissez la valeur de l’identificateur pour l’API Web. Pour cet exemple, il doit s’agir de la valeur de l' **URL SSL** dans laquelle l’application Web est supposée être en cours d’exécution. Vous pouvez récupérer cette valeur en cliquant sur les propriétés du projet TooListServer dans la solution. Ce sera utilisé ultérieurement comme valeur **TODO : TodoListResourceId** dans le fichier **app. config** de l’application cliente native et également comme **TODO : TodoListBaseAddress**.
![API web](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. Parcourez la **stratégie appliquer la Access Control** et **configurez les autorisations d’application** avec les valeurs par défaut en place. La page Résumé doit se présenter comme indiqué ci-dessous.
![Résumé](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)

Cliquez sur suivant, puis terminez l’Assistant.

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>Ajoutez la revendication NameIdentifier à la liste des revendications émises
L’application de démonstration utilise la valeur de la revendication NameIdentifier à différents emplacements. Contrairement à Azure AD, AD FS n’émet pas de revendication NameIdentifier par défaut. Par conséquent, nous devons ajouter une règle de revendication pour émettre la revendication NameIdentifier afin que l’application puisse utiliser la valeur correcte. Dans cet exemple, le nom donné de l’utilisateur est émis en tant que valeur NameIdentifier pour l’utilisateur dans le jeton.
Pour configurer la règle de revendication, ouvrez le groupe d’applications que vous venez de créer, puis double-cliquez sur l’API Web. Sélectionnez l’onglet Règles de transformation d’émission, puis cliquez sur le bouton Ajouter une règle. Dans la règle type de revendication, choisissez règle de revendication personnalisée, puis ajoutez la règle de revendication comme indiqué ci-dessous.

```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"), query = ";givenName;{0}", param = c.Value);
```

![Règle de revendication NameIdentifier](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>Modifier le code de l’application

Cette section explique comment télécharger l’exemple d’API Web et le modifier dans Visual Studio.   Nous allons utiliser l’exemple Azure AD qui se trouve [ici](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop).  

Pour télécharger l’exemple de projet, utilisez git bash et tapez la commande suivante :  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-native-desktop  
```  

#### <a name="modify-todolistclient"></a>Modifier ToDoListClient

Ce projet dans la solution représente l’application cliente native. Nous devons nous assurer que l’application cliente sait :

1. Où aller pour que l’utilisateur s’authentifie à la demande ?
2. Quel est l’ID que le client doit fournir à l’autorité d’authentification (AD FS) ?
3. Quel est l’ID de la ressource pour laquelle nous demandons le jeton d’accès ?
4. Quelle est l’adresse de base de l’API Web ?

Les modifications de code suivantes sont nécessaires pour récupérer les informations ci-dessus dans l’application cliente native.

**App. config**

* Ajoutez la clé **Ida : Authority** avec la valeur représentant le service AD FS. Par exemple, https://fs.contoso.com/adfs/.
* Modifiez la clé **Ida : ClientID** avec la valeur de l' **identificateur du client** dans la page **application native** lors de la création du groupe d’applications dans AD FS. Par exemple, 3f07368b-6efd-4F50-A330-d93853f4c855
* Modifiez la valeur **TODO : TODO : TodoListResourceId** avec la valeur de l' **identificateur** dans la page **configurer l’API Web** lors de la création du groupe d’applications dans AD FS. Par exemple, https://localhost:44321/.
* Modifiez la valeur **TODO : TodoListBaseAddress** avec la valeur de l' **identificateur** dans la page **configurer l’API Web** lors de la création du groupe d’applications dans AD FS. Par exemple, https://localhost:44321/.
* Définissez la valeur de **Ida : RedirectUri** avec la valeur de l' **URI de redirection** dans la page **application native** pendant la création du groupe d’applications dans AD FS. Par exemple, https://ToDoListClient.
* Pour faciliter la lecture, vous pouvez supprimer/commenter la clé pour **Ida : locataire** et **Ida : AADInstance**.

  ![Configuration de l’application](media/native-client-with-ad-fs-2016/app_configfile.PNG)


**MainWindow.xaml.cs**

* Commentez la ligne pour aadInstance comme ci-dessous

        // private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];

* Ajoutez la valeur de l’autorité comme ci-dessous

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* Supprimer la ligne de création de la valeur d' **autorité** de aadInstance et de locataire

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* Dans la fonction **MainWindow**, remplacez l’instanciation authContext par

        authContext = new AuthenticationContext(authority,false);

    ADAL ne prend pas en charge la validation de AD FS en tant qu’autorité et, par conséquent, nous devons passer un indicateur de valeur false pour le paramètre validateAuthority.

  ![Fenêtre principale](media/native-client-with-ad-fs-2016/mainwindow.PNG)

#### <a name="modify-todolistservice"></a>Modifier TodoListService
Deux fichiers doivent être modifiés dans ce projet (Web. config et Startup.Auth.cs). Les modifications apportées à Web. config sont nécessaires pour récupérer les valeurs correctes des paramètres. Les modifications apportées à Startup.Auth.cs sont nécessaires pour que les WebAPI s’authentifient par rapport aux AD FS plutôt que Azure AD.

**Web.config**

* Commentez la clé **Ida : locataire** comme nous n’en avons pas besoin
* Ajoutez la clé pour **Ida : Authority** avec une valeur indiquant le nom de domaine complet du service de Fédération, par exemple https://fs.contoso.com/adfs/
* Modifiez la clé **Ida : audience** avec la valeur de l’identificateur de l’API Web que vous avez spécifiée dans la page **configurer l’API Web** lors de l’ajout d’un groupe d’applications dans AD FS.
* Ajoutez la clé **Ida : AdfsMetadataEndpoint** avec une valeur correspondant à l’URL des métadonnées de Fédération du service AD FS, par exemple : https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

![config web](media/native-client-with-ad-fs-2016/webconfig.PNG)


**Startup.Auth.cs**

Modifiez la fonction ConfigureAuth comme ci-dessous

    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseActiveDirectoryFederationServicesBearerAuthentication(
            new ActiveDirectoryFederationServicesBearerAuthenticationOptions
            {
                MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
                TokenValidationParameters = new TokenValidationParameters()
                {
                    SaveSigninToken = true,
                    ValidAudience = ConfigurationManager.AppSettings["ida:Audience"]
                }

            });
    }

Pour l’essentiel, nous configurons l’authentification pour utiliser AD FS et fournissons davantage d’informations sur les métadonnées AD FS, et pour valider le jeton, la revendication d’audience doit être la valeur attendue par l’API Web.
Exécution de l’application

1. Sur la solution NativeClient-DotNet, cliquez avec le bouton droit et accédez à propriétés. Modifiez le projet de démarrage comme indiqué ci-dessous pour plusieurs projets de démarrage et définissez TodoListClient et TodoListService sur Start.
Propriétés de la solution ![](media/native-client-with-ad-fs-2016/solutionproperties.png)

2.  Appuyez sur le bouton F5 ou sélectionnez Déboguer > continuer dans la barre de menus. Cela permet de lancer l’application native et WebAPI. Cliquez sur le bouton de connexion dans l’application native pour ouvrir une session interactive à partir d’AD AL et rediriger vers votre service de AD FS. Entrez les informations d’identification d’un utilisateur valide.
![Connexion](media/native-client-with-ad-fs-2016/sign-in.png)

Dans cette étape, l’application native est redirigée vers AD FS et a obtenu un jeton d’ID et un jeton d’accès pour l’API Web.

3. Entrez un élément à faire dans la zone de texte, puis cliquez sur Ajouter un élément. Dans cette étape, l’application accède à l’API Web pour ajouter l’élément à faire, et pour ce faire, présente le jeton d’accès au WebAPI obtenu à partir de AD FS. L’API Web correspond à la valeur audience pour s’assurer que le jeton lui est destiné et vérifie la signature du jeton à l’aide des informations des métadonnées de Fédération.

![Connexion](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>Étapes suivantes
[Développement des services AD FS](../../ad-fs/AD-FS-Development.md)  
