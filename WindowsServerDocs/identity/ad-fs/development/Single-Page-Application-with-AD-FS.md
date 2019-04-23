---
title: Créer une application web à page unique à l’aide d’OAuth et la bibliothèque ADAL. JS avec AD FS 2016
description: Une procédure pas à pas qui fournit des instructions pour l’authentification auprès d’AD FS à l’aide de la bibliothèque ADAL pour JavaScript sécurisation une AngularJS en fonction d’application à page unique
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.openlocfilehash: 78ab9f5d7c3e75650a4efb171d3b9281c56c63d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865300"
---
# <a name="build-a-single-page-web-application-using-oauth-and-adaljs-with-ad-fs-2016"></a>Créer une application web à page unique à l’aide d’OAuth et la bibliothèque ADAL. JS avec AD FS 2016

>S'applique à : Windows Server 2016

Cette procédure pas à pas fournit des instructions pour l’authentification auprès d’AD FS à l’aide de la bibliothèque ADAL pour JavaScript sécurisation une AngularJS en fonction d’application à page unique, implémentée avec un serveur principal d’API Web ASP.NET.

Dans ce scénario, quand l’utilisateur se connecte, le code JavaScript frontal utilise [Active Directory Authentication Library pour JavaScript (la bibliothèque ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js) et l’octroi d’autorisation implicite pour obtenir un jeton d’ID (id_token) d’Azure AD. Le jeton est mis en cache et le client l’attache à la demande en tant que le jeton du porteur lors d’appels à son API Web principale, qui est sécurisé à l’aide de l’intergiciel (middleware) OWIN.

>AVERTISSEMENT : L’exemple que vous pouvez générer ici est à titre éducatif uniquement. Ces instructions concernent l’implémentation la plus simple, plus minimale possible d’exposer les éléments requis du modèle. L’exemple ne peut pas inclure tous les aspects de la gestion des erreurs et autres concernent les fonctionnalités.

>[!NOTE]
>Cette procédure pas à pas concerne **uniquement** pour le serveur AD FS 2016 et versions ultérieures 

## <a name="overview"></a>Vue d'ensemble
Dans cet exemple, nous allons créer un flux d’authentification dans lequel un client d’application monopage authentifiera dans AD FS pour sécuriser l’accès aux ressources sur le serveur principal WebAPI. Voici le flux d’authentification globale


![Autorisation de AD FS](media/Single-Page-Application-with-AD-FS/authenticationflow.PNG)

Lorsque vous utilisez une application à page unique, l’utilisateur permet d’accéder à un emplacement de départ à partir de cas à partir de la page et une collection de fichiers JavaScript et des vues HTML sont chargés. Vous devez configurer l’Active Directory Authentication Library (ADAL) pour connaître les informations critiques sur votre application, par exemple, l’instance AD FS, ID de client, afin qu’elle peut le diriger l’authentification auprès des services AD FS.

Si la bibliothèque ADAL voit un déclencheur pour l’authentification, il utilise les informations fournies par l’application et dirige l’authentification à votre service STS AD FS.  L’application à page unique, qui est enregistrée comme un client public dans AD FS, est automatiquement configurée pour le flux d’octroi implicite. Les résultats de demande d’autorisation dans un jeton d’ID qui est retourné à l’application via un #fragment. Les autres appels au serveur principal WebAPI contiendra ce jeton d’ID en tant que le jeton du porteur dans l’en-tête pour accéder à l’API Web.

## <a name="setting-up-the-development-box"></a>Configuration de la boîte de développement
Cette procédure pas à pas utilise Visual Studio 2015. Le projet utilise la bibliothèque ADAL JS. Pour en savoir plus sur la bibliothèque ADAL, consultez [.NET de bibliothèque d’authentification Active Directory.](https://msdn.microsoft.com/library/azure/mt417579.aspx)

## <a name="setting-up-the-environment"></a>Configuration de l’environnement
Pour cette procédure pas à pas, nous allons utiliser une configuration de base de :

1.  CONTRÔLEUR DE DOMAINE : Contrôleur de domaine pour le domaine dans lequel les services AD FS sera hébergée
2.  Serveur AD FS : Le serveur AD FS pour le domaine
3.  Ordinateur de développement : Ordinateur sur lequel nous Visual Studio est installé et que vous développerez notre exemple

Vous pouvez utiliser si vous le souhaitez, seuls deux machines. Une pour le contrôleur de domaine/AD FS et l’autre pour le développement de l’exemple.

Comment configurer le contrôleur de domaine et d’AD FS n’entre pas dans la portée de cet article. Pour des informations supplémentaires sur le déploiement, consultez :

- [Déploiement d’AD DS](../../ad-ds/deploy/AD-DS-Deployment.md) 
- [Déploiement d’AD FS](../AD-FS-Deployment.md)



## <a name="clone-or-download-this-repository"></a>Clonez ou téléchargez ce dépôt
Nous allons utiliser l’exemple d’application créé pour l’intégration d’Azure AD dans une application à page unique AngularJS puis de le modifier pour sécuriser à la place de la ressource principale à l’aide d’AD FS.

À partir de votre interpréteur de commandes ou de la ligne de commande :

    git clone https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp.git

## <a name="about-the-code"></a>À propos du Code
Les fichiers de clés contenant la logique d’authentification sont les suivantes :

**App.js** : injecte la dépendance de module ADAL, fournit les valeurs de configuration d’application utilisés par la bibliothèque ADAL permet d’optimiser les interactions de protocole avec AAD et indique les itinéraires qui ne doivent pas être accessibles sans authentification précédente.

**index.HTML** -contient une référence à adal.js

**HomeController.js**-montre comment tirer parti des méthodes login() et logOut() dans la bibliothèque ADAL.

**UserDataController.js** -montre comment extraire des informations utilisateur du jeton id_token mis en cache.

**Startup.Auth.cs** -contient la configuration de l’API Web à utiliser le Service de fédération Active Directory pour l’authentification du porteur.

## <a name="registering-the-public-client-in-ad-fs"></a>L’inscription du client public dans AD FS
Dans l’exemple, l’API Web est configuré pour écouter à https://localhost:44326/. Le groupe d’applications **navigateur Web l’accès à une application web** peut être utilisé pour la configuration d’application de flux d’octroi implicite.

1. Ouvrez la console de gestion AD FS, puis cliquez sur **ajouter l’Application groupe**. Dans le **Assistant Ajout d’un groupe Application** Entrez le nom de l’application, une description et sélectionnez le **navigateur Web l’accès à une application web** modèle à partir de la **Client-serveur applications** section comme indiqué ci-dessous  <br>![Créer le nouveau groupe d’applications](media/Single-Page-Application-with-AD-FS/appgroup_step1.png)

2. Dans la page suivante **application Native**, fournissez l’identificateur de client d’application et l’URI de redirection comme indiqué ci-dessous  <br>![Créer le nouveau groupe d’applications](media/Single-Page-Application-with-AD-FS/appgroup_step2.png)

3. Dans la page suivante **appliquer la stratégie de contrôle d’accès** laisser les autorisations en tant que *autoriser tout le monde*

4. La page de résumé doit se présenter comme ci-dessous  <br>![Créer le nouveau groupe d’applications](media/Single-Page-Application-with-AD-FS/appgroup_step3.png)

5. Cliquez sur **suivant** pour terminer l’ajout du groupe d’applications et de fermer l’Assistant.

## <a name="modifying-the-sample"></a>Modification de l’exemple
Configurer la bibliothèque ADAL JS

Ouvrez le **app.js** du fichier et changer la **adalProvider.init** définition pour :

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
|instance|URL de votre STS, par exemple, https://fs.contoso.com/
|tenant|Conserver en tant que « adfs »
|clientID|C’est l’ID de client que vous avez spécifié lors de la configuration du client public pour votre application à page unique

## <a name="configure-webapi-to-use-ad-fs"></a>Configurer les API Web pour utiliser AD FS
Ouvrez le **Startup.Auth.cs** dans l’exemple de fichier et ajoutez le code suivant au début : 

    using System.IdentityModel.Tokens;

Supprimer :

                app.UseWindowsAzureActiveDirectoryBearerAuthentication(
    new WindowsAzureActiveDirectoryBearerAuthenticationOptions
    {
    Audience = ConfigurationManager.AppSettings["ida:Audience"],
    Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
    });

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

|Paramètre|Description
|--------|--------
|ValidAudience|Cela permet de configurer la valeur de « audience » qui sera vérifiée par rapport à dans le jeton
|ValidIssuer|Cela permet de configurer la valeur de ' émetteur qui sera vérifié par rapport à dans le jeton
|MetadataEndpoint|Ce paramètre pointe vers les informations de métadonnées de votre STS

## <a name="add-application-configuration-for-ad-fs"></a>Ajouter la configuration de l’application pour AD FS
Modifiez l’appsettings comme indiqué ci-dessous :

    <appSettings>
    <add key="ida:Audience" value="https://localhost:44326/" />
    <add key="ida:AdfsMetadataEndpoint" value="https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml" />
    <add key="ida:Issuer" value="https://fs.contoso.com/adfs" />
      </appSettings>

## <a name="running-the-solution"></a>Exécution de la solution
Nettoyer la solution, régénérez la solution et l’exécuter. Si vous souhaitez afficher les traces détaillées, lancez Fiddler et activer le déchiffrement HTTPS.

Le navigateur se charge de l’application SPA et s’affiche l’écran suivant :

![Inscrire le client](media/Single-Page-Application-with-AD-FS/singleapp3.PNG)

Cliquez sur connexion.  La liste de tâches déclenchera le flux d’authentification et de JS ADAL dirigera l’authentification AD FS

![connexion](media/Single-Page-Application-with-AD-FS/singleapp4a.PNG)

Dans Fiddler, vous pouvez voir le jeton est retourné en tant que partie de l’URL dans le fragment de #.

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp5a.PNG)

Vous pourrez maintenant appeler l’API pour ajouter des éléments de liste de tâches pour l’utilisateur connecté principale :

![Fiddler](media/Single-Page-Application-with-AD-FS/singleapp6.PNG)

## <a name="next-steps"></a>Étapes suivantes
[Développement de AD FS](../../ad-fs/AD-FS-Development.md)  
