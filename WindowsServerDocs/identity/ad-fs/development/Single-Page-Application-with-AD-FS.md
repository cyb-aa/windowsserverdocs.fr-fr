---
title: "Créer une application web à page unique à l’aide d’OAuth et ADAL.JS avec ADFS2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 7b3d48e1e38baffeb84b1f236efb43cfda5048c0
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016"></a>Créer une application web à page unique à l’aide d’OAuth et ADAL.JS avec ADFS2016

>S’applique à: Windows Server2016

Cette procédure pas à pas fournit l’instruction d’authentification auprès d’ADFS à l’aide de ADAL pour JavaScript sécurisation une AngularJS application à page unique, implémentée avec une API Web ASP.NET le serveur principal.

>Avertissement: L’exemple que vous pouvez générer ici est à titre informatif uniquement. Ces instructions permettent de l’implémentation de la plus simple, plus minimale possible d’exposer les éléments requis du modèle. L’exemple ne peut-être pas inclure tous les aspects de la gestion des erreurs et d’autres se rapportent à la fonctionnalité.

>[!NOTE]
>Cette procédure pas à pas est applicable **uniquement** pour le serveur ADFS2016 et versions ultérieures 

## <a name="overview"></a>Vue d’ensemble
Dans cet exemple, nous allons créer un flux d’authentification dans laquelle un client d’application unique page authentifiera par rapport à ADFS pour sécuriser l’accès aux ressources WebAPI sur le serveur principal. Voici le flux d’authentification globale


![ADFS autorisation](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Lorsque vous utilisez une application à page unique, l’utilisateur veut accéder à un emplacement de départ, à partir de cas à partir de la page et une collection de fichiers JavaScript et vues HTML sont chargés. Vous devez configurer la bibliothèque d’authentification ActiveDirectory (ADAL) pour connaître les informations critiques relatives à votre application, autrement dit, l’instance ADFS, l’ID client, afin qu’il peut diriger l’authentification ADFS.

Si ADAL détecte un déclencheur pour l’authentification, il utilise les informations fournies par l’application et dirige l’authentification à votre service STS ADFS.  L’application à page unique, qui est enregistrée comme un client public dans ADFS, est automatiquement configurée pour le flux d’octroi implicite. Les résultats de demande d’autorisation dans un jeton d’ID est renvoyé à l’application via un #fragment. Autres appels vers le serveur principal WebAPI transitent ce jeton ID en tant que le jeton de support dans l’en-tête pour accéder à la WebAPI.

## <a name="setting-up-the-development-box"></a>Configuration de la zone de développement
Cette procédure utilise Visual Studio2015. Le projet utilise la bibliothèque ADAL JS. Pour en savoir plus sur ADAL, veuillez lire [bibliothèque de l’authentification ActiveDirectory .NET.](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>Configuration de l’environnement
Pour cette procédure pas à pas, nous allons utiliser une configuration de base:

1.  : Contrôleur de domaine Contrôleur de domaine pour le domaine dans lequel les services ADFS sera hébergé
2.  Le serveur ADFS: Le serveur ADFS pour le domaine
3.  Ordinateur de développement: Ordinateur où nous Visual Studio est installé et développement d’échantillon

Vous pouvez utiliser si vous le souhaitez, seuls deux ordinateurs virtuels. Une pour le contrôleur de domaine/ADFS et l’autre pour le développement de l’exemple.

Comment configurer le contrôleur de domaine et les services ADFS est dépassent le cadre de cet article. Pour des informations supplémentaires sur le déploiement, voir:

- [Déploiement des services ADDS](../../ad-ds/deploy/AD-DS-Deployment.md) 
- [Déploiement d’ADFS](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Dupliquer ou télécharger ce référentiel
Nous utiliserons l’exemple d’application créé pour l’intégration d’Azure AD dans une application de la même page AngularJS et en le modifiant pour sécuriser à la place la ressource principal à l’aide d’ADFS.

À partir de votre environnement ou de la ligne de commande:

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>Sur le Code
Les fichiers de clé qui contient la logique d’authentification sont les suivants:

**App.js** - injecte la dépendance ADAL module fournit les valeurs de configuration d’application utilisés par ADAL pour la conduite des interactions avec le protocole avec AAD et indique les itinéraires qui ne doivent pas être accessibles sans authentification précédente.

**index.HTML** -contient une référence à adal.js

**HomeController.js**-montre comment tirer parti des méthodes login() et logOut() dans ADAL.

**UserDataController.js** -montre comment extraire des informations de l’utilisateur de la mise en cache id_token.

**Startup.Auth.cs** -contient la configuration pour la WebAPI à utiliser ActiveDirectory Federation Services pour l’authentification du support.

## <a name="registering-the-public-client-in-ad-fs"></a>Enregistrer le client public dans ADFS
Dans l’exemple, le WebAPI est configuré pour qu’il écoute sur https://localhost:44326/. Le groupe d’applications **navigateur Web l’accès à une application web** peut être utilisé pour la configuration d’application de flux grant implicite.

1. Ouvrez la console de gestion ADFS, puis cliquez sur **ajouter un groupe d’Application**. Dans le **Assistant Ajout d’un groupe d’Application** Entrez le nom de l’application, description, puis sélectionnez le **navigateur Web l’accès à une application web** modèle à partir de la **applications Client-serveur** section comme indiqué ci-dessous
    <br>![Créer le nouveau groupe d’applications](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. Sur la page suivante **application Native**, fournissez l’identificateur de client d’application et rediriger les URI comme indiqué ci-dessous
    <br>![Créer le nouveau groupe d’applications](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. Sur la page suivante **appliquer la stratégie de contrôle d’accès** ne pas les autorisations en tant que *autoriser tout le monde*

4. La page Résumé doit se présenter comme ci-dessous
    <br>![Créer le nouveau groupe d’applications](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Cliquez sur **suivant** pour terminer l’ajout du groupe de l’application et fermer l’Assistant.

## <a name="modifying-the-sample"></a>Modification de l’exemple
Configurer ADAL JS

Ouvrez le **app.js** de fichiers et de modifier le **adalProvider.init** définition de:

    adalProvider.init(
        {
            instance: 'https://fs.contoso.com/', // your STS URL
            tenant: 'adfs',                      // this should be adfs
            clientId: 'https://localhost:44326/', // your client ID of the
            //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
        },
        $httpProvider
        );

|Configuration|Description
|--------|--------
|instance|URL de service STS, par exemple, https://fs.contoso.com/
|client|Conserver en tant que «adfs»
|clientID|Il s’agit de l’ID de client que vous avez spécifié lors de la configuration du client public pour votre application à page unique

## <a name="configure-webapi-to-use-ad-fs"></a>Configurer WebAPI pour utiliser les services ADFS
Ouvrez le **Startup.Auth.cs** dans l’exemple de fichier et ajoutez le code suivant au début: 

    using System.IdentityModel.Tokens;

Supprimer:

                app.UseWindowsAzureActiveDirectoryBearerAuthentication(
    new WindowsAzureActiveDirectoryBearerAuthenticationOptions
    {
    Audience = ConfigurationManager.AppSettings["ida:Audience"],
    Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
    });

Et ajoutez:

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

|Paramètre|Description
|--------|--------
|ValidAudience|Cela configure la valeur de «public» qui est vérifié par rapport à dans le jeton.
|ValidIssuer|Cela configure la valeur de «émetteur vérifié dans le jeton.
|MetadataEndpoint|Il pointe vers les informations de métadonnées de votre service STS

## <a name="add-application-configuration-for-ad-fs"></a>Ajouter la configuration de l’application pour ADFS
Modifiez l’appsettings comme indiqué ci-dessous:

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>Exécution de la solution
Nettoyer la solution, régénérez la solution et l’exécuter. Si vous souhaitez voir traces détaillées, lancez Fiddler, puis activer le déchiffrement HTTPS.

Le navigateur se charge SPA et s’affiche avec l’écran suivant:

![Inscrire le client](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Cliquez sur connexion.  La liste ToDo déclenchera le flux d’authentification et JS ADAL dirigera l’authentification ADFS

![Ouverture de session](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

Dans Fiddler, vous pouvez voir le jeton est retourné dans le cadre de l’URL dans le fragment #.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

Vous pourrez maintenant appeler le API pour ajouter des éléments de liste ToDo pour l’utilisateur connecté principal:

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>Étapes suivantes
[Développement d’ADFS](../../ad-fs/AD-FS-Development.md)  
