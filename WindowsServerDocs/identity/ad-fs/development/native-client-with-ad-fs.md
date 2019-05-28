---
title: Générer un client natif application à l’aide des clients publics OAuth avec AD FS 2016 ou version ultérieur
description: Une procédure pas à pas qui fournit des instructions de la création d’un client natif application à l’aide des clients publics OAuth et AD FS 2016 ou version ultérieur
author: billmath
ms.author: billmath
ms.reviewer: anandy
manager: mtillman
ms.date: 07/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: e85a97fa08e4c77588b17aee08ee03e0b897a74c
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976847"
---
# <a name="build-a-native-client-application-using-oauth-public-clients-with-ad-fs-2016-or-later"></a>Générer un client natif application à l’aide des clients publics OAuth avec AD FS 2016 ou version ultérieur

## <a name="overview"></a>Vue d'ensemble

Cet article explique comment créer une application native qui interagit avec une API Web protégées par AD FS 2016 ou version ultérieure.

1. Le .net application TodoListClient WPF utilise la bibliothèque d’authentification Active Directory (ADAL) pour obtenir un jeton d’accès JWT d’Azure Active Directory (Azure AD) via le protocole OAuth 2.0
2. Le jeton d’accès est utilisé comme un jeton du porteur pour authentifier l’utilisateur lors de l’appel /todolist point de terminaison de l’API de web TodoListService.
 Nous être à l’aide de l’exemple d’application pour Azure AD ici et modifiez-la ensuite pour AD FS 2016 ou version ultérieure.

![Présentation de l’application](media/native-client-with-ad-fs-2016/appoverview.png)

## <a name="pre-requisites"></a>Conditions préalables
Voici une liste de conditions préalables qui sont nécessaires avant la fin de ce document. Ce document suppose que AD FS a été installé et une batterie de serveurs AD FS a été créé.

* Outils clients de GitHub
* AD FS dans Windows Server 2016 ou version ultérieure
* Visual Studio 2013 ou version ultérieure

## <a name="creating-the-sample-walkthrough"></a>Création de l’exemple de procédure

### <a name="create-the-application-group-in-ad-fs"></a>Créer le groupe d’applications dans AD FS

1. Dans Gestion AD FS, cliquez sur **groupes d’applications** et sélectionnez **ajouter l’Application groupe**.

2. Dans l’Assistant de groupe d’Application, pour le nom, entrez n’importe quel nom que vous préférez, par exemple, NativeToDoListAppGroup. Sélectionnez le **application Native accédant à une API web** modèle. Cliquez sur **Suivant**.
 ![Ajouter le groupe d’applications](media/native-client-with-ad-fs-2016/addapplicationgroup1.png)

3. Sur le **application Native** page, notez l’identificateur généré par AD FS. Il s’agit de l’id avec lequel AD FS reconnaîtra l’application cliente publique. Copie le **identificateur Client** valeur. Il sera utilisé ultérieurement comme valeur pour **ida : ClientId** dans le code d’application. Si vous le souhaitez, vous pouvez donner n’importe quel identificateur personnalisé ici. L’URI de redirection est n’importe quelle valeur arbitraire, exemple, placez https://ToDoListClient ![application Native](media/native-client-with-ad-fs-2016/addapplicationgroup2.png)

4. Sur le **configurer des API Web** , définissez la valeur d’identificateur pour l’API Web. Pour cet exemple, cela doit être la valeur de la **URL SSL** où l’application Web est censé être en cours d’exécution. Vous pouvez obtenir cette valeur en cliquant sur les propriétés du projet TooListServer dans la solution. Cela sera utilisé ultérieurement comme le **todo:TodoListResourceId** valeur dans **App.config** fichier de l’application cliente native, ainsi qu’en le **todo:TodoListBaseAddress**.
![API Web](media/native-client-with-ad-fs-2016/addapplicationgroup3.png)

5. Passez en revue la **appliquer la stratégie de contrôle d’accès** et **configurer les autorisations Application** avec les valeurs par défaut en place. La page de résumé doit se présenter comme ci-dessous.
![Résumé](media/native-client-with-ad-fs-2016/addapplicationgroupsummary.png)

Cliquez sur Suivant, puis terminez l’Assistant.

### <a name="add-the-nameidentifier-claim-to-the-list-of-claims-issued"></a>Ajoutez la revendication NameIdentifier à la liste des revendications émises
L’application de démonstration utilise la valeur de revendication NameIdentifier à divers emplacements. Contrairement à Azure AD, AD FS n’émet pas d’une revendication NameIdentifier par défaut. Par conséquent, nous devons ajouter une règle de revendication pour émettre la revendication NameIdentifier afin que l’application peut utiliser la valeur correcte. Dans cet exemple, le prénom de l’utilisateur est émis en tant que la valeur NameIdentifier de l’utilisateur dans le jeton.
Pour configurer la règle de revendication, ouvrez le groupe d’applications venez de créer et double-cliquez sur l’API Web. Sélectionnez l’onglet Règles de transformation d’émission, puis sur Ajouter une règle. Dans le type de règle de revendication, choisissez la règle de revendication personnalisée, puis ajoutez la règle de revendication comme indiqué ci-dessous.

```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]
 => issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier"), query = ";givenName;{0}", param = c.Value);
```

![Règle de revendication NameIdentifier](media/native-client-with-ad-fs-2016/addnameidentifierclaimrule.png)

### <a name="modify-the-application-code"></a>Modifier le code d’application

Cette section explique comment télécharger l’exemple d’API Web et la modifier dans Visual Studio.   Nous allons utiliser l’exemple d’Azure AD est [ici](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop).  

Pour télécharger l’exemple de projet, utilisez Git Bash et tapez la commande suivante :  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-native-desktop  
```  

#### <a name="modify-todolistclient"></a>Modifier ToDoListClient

Ce projet dans la solution représente l’application cliente native. Nous avons besoin pour vous assurer que l’application cliente sait :

1. Où aller pour obtenir de l’utilisateur authentifié à la demande ?
2. Quel est l’ID de ce client doit fournir à l’autorité d’authentification (AD FS) ?
3. Quel est l’ID de la ressource que nous demandons le jeton d’accès ?
4. Qu’est l’adresse de base de l’API Web ?

Les modifications de code suivantes sont nécessaires afin d’obtenir les informations ci-dessus à l’application cliente native.

**App.config**

* Ajouter la clé **ida : autorité** avec la valeur représentant le service AD FS. Par exemple, https://fs.contoso.com/adfs/.
* Modifier **ida : ClientId** clé avec la valeur de **identificateur Client** dans le **Application Native** page lors de la création du groupe d’applications dans AD FS. Par exemple, 3f07368b-6efd-4f50-a330-d93853f4c855
* Modifier le **todo:todo:TodoListResourceId** avec la valeur de **identificateur** dans le **configurer des API Web** page lors de la création du groupe d’applications dans AD FS. Par exemple, https://localhost:44321/.
* Modifier le **todo:TodoListBaseAddress** avec la valeur de **identificateur** dans le **configurer des API Web** page lors de la création du groupe d’applications dans AD FS. Par exemple, https://localhost:44321/.
* Définissez la valeur de **ida : RedirectUri** avec la valeur de **URI de redirection** dans le **application Native** page lors de la création du groupe d’applications dans AD FS. Par exemple, https://ToDoListClient.
* Pour faciliter la lecture, vous pouvez supprimer ou commenter la clé pour **ida : client** et **ida : AADInstance**.

  ![Configuration de l’application](media/native-client-with-ad-fs-2016/app_configfile.PNG)


**MainWindow.xaml.cs**

* Commentez la ligne pour aadInstance comme indiqué ci-dessous

        // private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];

* Ajouter la valeur de l’autorité comme indiqué ci-dessous

        private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

* Supprimez la ligne pour la création de la **autorité** des aadInstance et locataire

        private static string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);

* Dans la fonction **MainWindow**, modifiez l’instanciation authContext à

        authContext = new AuthenticationContext(authority,false);

    La bibliothèque ADAL ne prend pas en charge la validation AD FS en tant qu’autorité et par conséquent, nous devons passer d’un indicateur de la valeur false pour le paramètre de validateAuthority.

  ![Fenêtre principale](media/native-client-with-ad-fs-2016/mainwindow.PNG)

#### <a name="modify-todolistservice"></a>Modifier TodoListService
Deux fichiers doivent être modifiés dans ce projet : Web.config et Startup.Auth.cs. Modifications de Web.Config sont nécessaires pour obtenir les valeurs correctes des paramètres. Modifications de Startup.Auth.cs sont nécessaires pour définir l’API Web pour s’authentifier avec AD FS plutôt qu’Azure AD.

**Web.config**

* Commentaire de la clé **ida : client** que nous n’en avez besoin
* Ajoutez la clé de **ida : autorité** avec une valeur qui indique le nom de domaine complet de la fédération, exemple, de service https://fs.contoso.com/adfs/
* Modifier la clé **ida : Audience** avec la valeur de l’identificateur de l’API Web que vous avez spécifié dans le **configurer des API Web** page au cours d’ajouter l’Application de groupe dans AD FS.
* Ajouter une clé **ida : AdfsMetadataEndpoint** avec une valeur correspondant à l’URL de métadonnées de fédération d’ADFS service, par exemple : https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

![Configuration Web](media/native-client-with-ad-fs-2016/webconfig.PNG)


**Startup.Auth.cs**

Modifiez la fonction ConfigureAuth comme indiqué ci-dessous

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

En fait, nous allons configurer l’authentification à utiliser les services AD FS et fournir davantage d’informations sur les métadonnées d’AD FS et pour valider le jeton, la revendication d’audience doit être la valeur attendue par l’API Web.
Exécution de l’application

1. Sur la solution NativeClient-DotNet, avec le bouton droit cliquez et accédez aux propriétés. Modifier le projet de démarrage comme indiqué ci-dessous pour les projets de démarrage multiples et définir le démarrage TodoListClient et TodoListService.
![Propriétés de la solution](media/native-client-with-ad-fs-2016/solutionproperties.png)

2.  Appuyez sur la touche F5 ou sélectionnez Déboguer > Continuer dans la barre de menus. Cette action lance l’application native et l’API Web. Cliquez sur le bouton de connexion sur l’application native et il fenêtre contextuelle une ouverture de session interactive à partir d’AD AL et rediriger vers votre service AD FS. Entrez les informations d’identification d’un utilisateur valide.
![Connectez-vous](media/native-client-with-ad-fs-2016/sign-in.png)

Dans cette étape, l’application native redirigé vers AD FS et obtenu un jeton d’ID et un jeton d’accès pour l’API Web

3.  Entrez un élément dans la zone de texte et cliquez sur Ajouter un élément. Dans cette étape, l’application contacte l’API Web pour ajouter l’élément de tâche et pour ce faire, présentent le jeton d’accès à l’API Web obtenu à partir d’AD FS. L’API Web correspond à la valeur de l’audience pour vous assurer que le jeton est destiné et vérifie la signature du jeton en utilisant les informations à partir des métadonnées de fédération.

![Connexion](media/native-client-with-ad-fs-2016/clienttodoadd.png)

## <a name="next-steps"></a>Étapes suivantes
[Développement des services AD FS](../../ad-fs/AD-FS-Development.md)  
