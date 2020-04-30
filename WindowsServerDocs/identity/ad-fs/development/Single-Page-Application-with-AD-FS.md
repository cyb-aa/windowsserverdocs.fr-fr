---
title: Générez une application Web à page unique à l’aide de OAuth et ADAL. JS avec AD FS 2016 ou version ultérieure
description: Procédure pas à pas fournissant des instructions pour l’authentification par rapport à AD FS à l’aide de ADAL pour JavaScript sécurisation d’une application à page unique basée sur AngularJS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/13/2018
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.openlocfilehash: f62b6ad288e2733083d535260f0b3f5ffb5b50bf
ms.sourcegitcommit: f829a48b9b0c7b9ed6e181b37be828230c80fb8a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2020
ms.locfileid: "82173622"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016-or-later"></a>Générez une application Web à page unique à l’aide de OAuth et ADAL. JS avec AD FS 2016 ou version ultérieure

Cette procédure pas à pas fournit des instructions pour l’authentification par rapport à AD FS à l’aide de ADAL pour la sécurisation d’une application à page unique basée sur AngularJS, implémentée avec un backend API Web ASP.NET.

Dans ce scénario, quand l’utilisateur se connecte, le JavaScript frontal utilise [Active Directory Authentication Library pour JavaScript (ADAL.JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) et l’octroi d’autorisation implicite pour obtenir un jeton d’ID (id_token) d’Azure AD. Le jeton est mis en cache, et le client l’attache à la demande en tant que jeton porteur lors de l’appel des composants principaux de son API web, qui sont sécurisés à l’aide de l’intergiciel OWIN.

>[!IMPORTANT]
>L’exemple que vous pouvez générer ici est fourni à titre éducatif uniquement. Ces instructions sont destinées à l’implémentation la plus simple et la plus minimale possible pour exposer les éléments requis du modèle. L’exemple peut ne pas inclure tous les aspects de la gestion des erreurs et d’autres fonctionnalités associées.

>[!NOTE]
>Cette procédure pas à pas s’applique **uniquement** aux AD FS Server 2016 et versions ultérieures. 

## <a name="overview"></a>Vue d’ensemble
Dans cet exemple, nous allons créer un workflow d’authentification où un client d’application à page unique s’authentifiera sur AD FS pour sécuriser l’accès aux ressources WebAPI sur le serveur principal. Voici le déroulement global de l’authentification


![Autorisation AD FS](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Lorsque vous utilisez une application à page unique, l’utilisateur accède à un emplacement de départ, à partir duquel la page de démarrage et une collection de fichiers JavaScript et de vues HTML sont chargées. Vous devez configurer le Bibliothèque d’authentification Active Directory (ADAL) pour connaître les informations critiques sur votre application, c’est-à-dire l’instance de AD FS, l’ID client, afin qu’elle puisse diriger l’authentification vers votre AD FS.

Si ADAL voit un déclencheur pour l’authentification, il utilise les informations fournies par l’application et dirige l’authentification vers votre STS AD FS.  L’application à page unique, qui est inscrite en tant que client public dans AD FS, est automatiquement configurée pour le workflow d’octroi implicite. La demande d’autorisation génère un jeton d’ID renvoyé à l’application via un #fragment. D’autres appels à la WebAPI backend comporteront ce jeton d’ID en tant que jeton du porteur dans l’en-tête pour accéder au WebAPI.

## <a name="setting-up-the-development-box"></a>Configuration de la zone de développement
Cette procédure pas à pas utilise Visual Studio 2015. Le projet utilise la bibliothèque ADAL JS. Pour en savoir plus sur ADAL, consultez [bibliothèque d’Authentification Active Directory .net.](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>Configuration de l’environnement
Pour cette procédure pas à pas, nous allons utiliser une configuration de base de :

1.    DC : contrôleur de domaine pour le domaine dans lequel AD FS sera hébergé
2.    Serveur de AD FS : serveur AD FS pour le domaine
3.    Ordinateur de développement : ordinateur sur lequel Visual Studio est installé et qui développera notre exemple

Si vous le souhaitez, vous pouvez utiliser uniquement deux ordinateurs. Une pour DC/AD FS et l’autre pour le développement de l’exemple.

La configuration du contrôleur de domaine et du AD FS dépasse le cadre de cet article. Pour plus d’informations sur le déploiement, consultez :

- [Déploiement AD DS](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Déploiement d’AD FS](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Cloner ou télécharger ce dépôt
Nous allons utiliser l’exemple d’application créé pour intégrer Azure AD dans une application à page unique AngularJS et la modifier pour sécuriser la ressource principale à l’aide de AD FS.

À partir de votre interpréteur de commandes ou de votre ligne de commande :

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>À propos du code
Les fichiers de clé contenant la logique d’authentification sont les suivants :

**App. js** : injecte la dépendance du module Adal, fournit les valeurs de configuration de l’application utilisées par Adal pour la conduite des interactions de protocole avec AAD et indique les itinéraires qui ne sont pas accessibles sans authentification antérieure.

**index. html** -contient une référence à Adal. js

**HomeController. js**-montre comment tirer parti des méthodes login () et logOut () dans Adal.

**UserDataController. js** -indique comment extraire les informations utilisateur du id_token mis en cache.

**Startup.auth.cs** : contient la configuration de WebAPI à utiliser Active Directory Service FS (Federation Service) pour l’authentification du porteur.

## <a name="registering-the-public-client-in-ad-fs"></a>Inscription du client public dans AD FS
Dans l’exemple, WebAPI est configuré pour écouter https://localhost:44326/. Le navigateur Web de groupe d’applications qui **accède à une application Web** peut être utilisé pour configurer l’application d’octroi de fluide implicite.

1. Ouvrez la console de gestion AD FS, puis cliquez sur **Ajouter un groupe d’applications**. Dans l' **Assistant Ajout** d’un groupe d’applications, entrez le nom de l’application, la description, puis sélectionnez le **navigateur Web accédant à un modèle d’application Web** à partir de la section **applications client-serveur** , comme indiqué ci-dessous.

    ![Créer un nouveau groupe d’applications](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. Dans l' **application native**page suivante, fournissez l’identificateur du client d’application et l’URI de redirection, comme indiqué ci-dessous

    ![Créer un nouveau groupe d’applications](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. Sur la page suivante, **appliquer la stratégie de Access Control** conserver les autorisations en tant que *autoriser tout le monde*

4. La page de résumé doit ressembler à ce qui suit

    ![Créer un nouveau groupe d’applications](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Cliquez sur **suivant** pour terminer l’ajout du groupe d’applications et fermer l’Assistant.

## <a name="modifying-the-sample"></a>Modification de l’exemple
Configurer ADAL JS

Ouvrez le fichier **app. js** et remplacez la définition **adalProvider. init** par :

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
    );

|Configuration|Description|
|--------|--------|
|instance|Votre URL STS, par exemplehttps://fs.contoso.com/|
|tenant|Conserver « ADFS »|
|clientID|Il s’agit de l’ID client que vous avez spécifié lors de la configuration du client public pour votre application à page unique|

## <a name="configure-webapi-to-use-ad-fs"></a>Configurer WebAPI pour utiliser AD FS
Ouvrez le fichier **Startup.auth.cs** dans l’exemple, puis ajoutez le code suivant au début :

    using System.IdentityModel.Tokens;

Installez

    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        }
    );

et ajoutez :

    app.UseActiveDirectoryFederationServicesBearerAuthentication(
        new ActiveDirectoryFederationServicesBearerAuthenticationOptions
        {
            MetadataEndpoint = ConfigurationManager.AppSettings["ida:AdfsMetadataEndpoint"],
            TokenValidationParameters = new TokenValidationParameters()
            {
                ValidAudience = ConfigurationManager.AppSettings["ida:Audience"],
                ValidIssuer = ConfigurationManager.AppSettings["ida:Issuer"]
            }
        }
    );

|Paramètre|Description|
|--------|--------|
|ValidAudience|Cela configure la valeur de « audience » qui sera vérifiée dans le jeton.|
|ValidIssuer|Cela permet de configurer la valeur de l’émetteur à vérifier dans le jeton.|
|MetadataEndpoint|Cela pointe vers les informations de métadonnées de votre STS.|

## <a name="add-application-configuration-for-ad-fs"></a>Ajouter une configuration d’application pour AD FS
Modifiez le appSettings comme indiqué ci-dessous :
```xml
    <appSettings>
        <add key="ida:Audience" value="https://localhost:44326/" />
        <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
        <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
    </appSettings>
    ```

## Running the solution
Clean the solution, rebuild the solution and run it. If you want to see detailed traces, launch Fiddler and enable HTTPS decryption.

The browser (use Chrome browser) will load the SPA and you will be presented with the following screen:

![Register the client](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Click on Login.  The ToDo List will trigger the authentication flow and ADAL JS will direct the authentication to AD FS

![Login](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

In Fiddler you can see the token being returned as part of the URL in the # fragment.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

You will be able to now call the backend API to add ToDo List items for the logged-in user:

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## Next Steps
[AD FS Development](../../ad-fs/AD-FS-Development.md)  
