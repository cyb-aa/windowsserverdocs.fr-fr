---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: "Créer une application à plusieurs niveaux à l’aide de On-Behalf-Of (OBO) à l’aide d’OAuth avec ADFS2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8940cde2b78ce3ead499263e6fba0fbe28aae695
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016"></a>Créer une application à plusieurs niveaux à l’aide de On-Behalf-Of (OBO) à l’aide d’OAuth avec ADFS2016

>S’applique à: Windows Server2016

Cette procédure pas à pas fournit des instructions pour l’implémentation d’une authentification (OBO) de la part de l’utilisation d’ADFS dans Windows Server2016 TP5.  O pour en savoir plus sur l’authentification OBO lisez [scénarios ADFS pour les développeurs](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)

>Avertissement: L’exemple que vous pouvez générer ici est à titre informatif uniquement. Ces instructions permettent de l’implémentation de la plus simple, plus minimale possible d’exposer les éléments requis du modèle. L’exemple ne peut-être pas inclure tous les aspects de la gestion des erreurs et d’autres sont liées à la fonctionnalité et se concentre uniquement l’obtention d’une authentification OBO réussie.

## <a name="overview"></a>Vue d’ensemble

Dans cet exemple, nous allons créer un flux d’authentification où un client accédant à un Service Web de couche intermédiaire et le service web sera puis agir au nom du client pour obtenir un jeton d’accès authentifié.

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.PNG)

Vous trouverez ci-dessous l’exemple permettra d’atteindre le flux d’authentification
1. Client s’authentifie auprès d’un point de terminaison ADFS d’autorisation et demande un code d’autorisation
2. Point de terminaison d’autorisation renvoie le code d’authentification client
3. Utilise le code d’authentification de client et il présente au point de terminaison jeton ADFS pour le jeton d’accès demande du Service Web de couche comme WebAPI
4. ADFS retourne le jeton d’accès au Service Web de couche intermédiaire. Pour des fonctionnalités supplémentaires, Service de couche intermédiaire doit accéder pour la Backend WebAPI
5. Client utilise le jeton d’accès pour utiliser le service de couche intermédiaire.
6. Service de couche intermédiaire fournit le jeton d’accès pour le point de terminaison jeton ADFS et demandes de jeton d’accès pour Backend WebAPI sur à la place de l’utilisateur authentifié
7. ADFS retourne le jeton d’accès pour le serveur principal WebAPI au Service de couche intermédiaire actiing en tant que client
8. Service de couche intermédiaire utilise le jeton d’accès fourni par ADFS à l’étape7 pour le système principal WebAPI en tant que client d’accès et exécuter les fonctions requises

## <a name="sample-structure"></a>Exemple de Structure

Exemple de composant de trois modules


Module | Description
-------|------------
ToDoClient | Client natif avec lequel l’utilisateur interagit
ToDoService | Web couche API qui agit en tant que client pour le système principal WebAPI au centre
WebAPIOBO | Principal web d’api qui est utilisé par ToDoService pour effectuer l’opération requise lorsque l’utilisateur ajoute un ToDoItem




## <a name="setting-up-the-development-box"></a>Configuration de la zone de développement

Cette procédure utilise Visual Studio2015. Le projet utilise de manière intensive bibliothèque de l’authentification ActiveDirectory (ADAL). Pour en savoir plus sur ADAL, veuillez lire [bibliothèque de l’authentification ActiveDirectory .NET](https://msdn.microsoft.com/library/azure/mt417579.aspx)

L’exemple utilise également v11.0 SQL LocalDB. Installez le SQL LocalDB avant l’utilisation de l’exemple.

## <a name="setting-up-the-environment"></a>Configuration de l’environnement
Nous travaillerons avec une configuration de base:

1. **Contrôleur de domaine**: contrôleur de domaine pour le domaine dans lequel les services ADFS sera hébergé
2. **Serveur ADFS**: le serveur ADFS pour le domaine
3. **Ordinateur de développement**: ordinateur sur lequel nous Visual Studio est installé et développement d’échantillon

Vous pouvez utiliser si vous le souhaitez, seuls deux ordinateurs virtuels. Une pour le contrôleur de domaine/ADFS et l’autre pour le développement de l’exemple.

Comment configurer le contrôleur de domaine et les services ADFS est dépassent le cadre de cet article. Pour des informations supplémentaires sur le déploiement, voir:

- [Déploiement des services ADDS](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Déploiement d’ADFS](../AD-FS-Deployment.md)

L’exemple se base sur l’exemple OBO existant par rapport à Azure créé par Vittorio Bertocci et disponible [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof). Suivez les instructions de cloner le projet sur votre ordinateur de développement et de créer une copie de l’exemple pour commencer à utiliser.

## <a name="clone-or-download-this-repository"></a>Dupliquer ou télécharger ce référentiel

À partir de votre environnement ou de la ligne de commande:

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>Modification de l’exemple

Dès que vous ouvrez la solution WebAPI-OnBehalfOf-DotNet.sln, vous remarquerez que vous disposez de deux projets dans la solution-

* **ToDoListClient**: cette valeur sera définie en tant que le client OpenID qui vont interagir avec l’utilisateur
* **ToDoListService**: cela est l’application de serveur Web intermédiaire / de Service qui vont interagir avec un autre serveur principal WebAPI OBO l’utilisateur authentifié

Comme vous pouvez le constater, nous devrons ajouter un autre projet ultérieurement qui jouera le rôle de la ressource qui sera accessible par le ToDoListService de couche intermédiaire.

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>La configuration ADFS pour le Client et l’application de serveur Web

Dans l’écran actuel de l’échantillon, l’authentification est configurée pour être effectuée par rapport à Azure AD. Nous voulons modifier le mécanisme d’authentification et direct il vers ADFS déployé sur site. Pour ce faire, nous devons configurer ADFS pour reconnaître le client et l’application serveur Web nous avons dans l’exemple.

**Création d’un groupe d’applications**

Ouvrez la console MMC de gestion ADFS et ajouter un nouveau groupe de l’application. Sélectionnez le modèle natif-Application-WebAPI.

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

Cliquez sur Suivant et s’affiche avec la page pour fournir des informations sur l’application cliente. Donnez un nom approprié pour le client App dans ADFS. Copiez l’identificateur de client et enregistrer un emplacement que vous pouvez accéder ultérieurement lorsque cela sera nécessaire dans la configuration de l’application dans visual studio.

>Remarque: L’URI de redirection peut être n’importe quel URI arbitraire car elle ne sert pas vraiment en cas de clients natifs

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

Cliquez sur Suivant et s’affiche avec la page pour fournir des informations sur les WebAPI. Donnez un nom approprié pour l’entrée d’ADFS pour le WebAPI et entrez l’URI de redirection en tant que l’URI que vous consultez dans Visual Studio pour le ToDoListService

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

Cliquez sur Suivant, et vous verrez la Page Choisir la stratégie de contrôle d’accès. Vérifiez que vous voyez «Autoriser tout le monde» dans la section stratégie.

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

Cliquez sur Suivant et s’affiche avec la page autorisations d’Application de configurer. Sur cette page, sélectionnez les étendues autorisés comme openid (sélectionné par défaut) et user_impersonation. L’étendue 'user_impersonation' est nécessaire pour pouvoir correctement demander un jeton d’accès de la part d’auprès d’ADFS.

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

Cliquez sur suivant affiche la page de résumé. Passez en revue le reste de l’Assistant et terminer la configuration.

Afin d’activer sur à la place de l’authentification, nous devons garantir que les services ADFS renvoie un jeton d’accès avec une étendue user_impersonation au client. Modifiez l’émission de revendications pour ToDoListServiceWebApi inclure les trois règles personnalisées suivantes:

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue open id scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "openid");

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**Ajout de ToDoListService en tant que client dans le groupe d’applications**

À ce stade, nous devons effectuer une entrée supplémentaire dans ADFS pour l’application de serveur Web d’agir en tant que client et pas seulement en tant que ressource. Ouvrez le groupe d’applications que vous venez de créer et cliquez sur Ajouter une Application.

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

La page «Ajouter une nouvelle application à MySampleGroup» s’affiche. Sur cette page, sélectionnez «Application ou site Web du serveur» en tant que l’application autonome

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

Cliquez sur Suivant et s’affiche avec la page de fournir des détails de l’application. Fournir un nom approprié pour l’entrée de configuration dans la section de nom. Assurez-vous que l’identificateur de Client est identique à l’identificateur de la ToDoListServiceWebAPI

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

Cliquez sur Suivant et s’affiche avec la page pour configurer les informations d’identification de l’application. Cliquez sur «Générer un secret partagé». Un secret qui est généré automatiquement s’affiche. Copiez le code secret à un emplacement car il sera requis pendant que nous configurer le ToDoListService dans visual studio.


![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

Cliquez sur Suivant, puis terminez l’Assistant.

### <a name="modifying-the-todolistclient-code"></a>Modification du code ToDoListClient

#### <a name="modify-the-application-config"></a>Modifier la configuration de l’Application

Accédez à vous êtes le projet ToDoListClient dans WebAPI-OnBehalfOf-DotNet solution. Ouvrez le fichier App.config et apporter les modifications suivantes

* Commentaire l’entrée de clé ida: client
* Pour ida: RedirectURI, entrez l’URI arbitraire que vous avez fournie lors de la configuration de la MySampleGroup_ClientApplication dans ADFS.
* Pour la clé: ClientID ida, fournissez au client identifiant ADFS a donné lors de la configuration de la MySampleGroup_ClientApplication.
* Pour l’ida: ToDoListResourceID fournir l’ID de ressource que vous avez attribué lors de la configuration de la ToDoListServiceWebApi dans ADFS
* Commentaire la clé ida: AADInstance
* Entrez l’ID de ressource de la ToDoListServiceWebApi l’ida: ToDoListBaseAddress. Il sera utilisé lors de l’appel du ToDoList WebAPI.
* Ajouter une clé ida: autorité et fournir la valeur en tant que l’URI d’ADFS.

Votre **appSettings** dans App.Config doit se présenter comme suit:

    <appSettings>
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />-->
    <add key="ida:ClientId" value="c7f7b85c-497c-4589-877f-b17a0bd13398" />
    <add key="ida:RedirectUri" value="https://arbitraryuri.com/" />
    <add key="ida:TodoListResourceId" value="https://localhost:44321/" />
    <!--<add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->
    <add key="ida:TodoListBaseAddress" value="https://localhost:44321" />
    <add key="ida:Authority" value="https://fs.anandmsft.com/adfs/"/>
    </appSettings>

#### <a name="modifying-the-code"></a>Modification du code

**MainWindow.xaml.cs**

Commentez la ligne lit les informations de client à partir de la configuration de l’application

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

Modifiez la valeur de l’autorité de chaîne pour

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

Modifier le code pour lire les valeurs correctes des ToDoListResourceId et ToDoListBaseAddress

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

Dans la fonction MainWindow() modifier l’initialisation authcontext en tant que:

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>Ajout de la ressource du serveur principal

Afin d’exécuter le flux de la part de, vous devez créer une ressource principal accédant à la ToDoListService sur à la place de l’utilisateur authentifié. Le choix de la ressource principal peut varier en fonction de la configuration requise, mais dans le cadre de cet exemple, vous pouvez créer une base WebAPI.

* Cliquez avec le bouton droit sur la solution 'WebAPI-OnBehalfOf-DotNet' dans l’Explorateur de solutions et sélectionnez Ajouter -> nouveau projet
* Choisissez le modèle d’Application Web ASP.NET

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* Sur la prochaine invite, cliquez sur «Modifier l’authentification»
* Sélectionnez «Travail et comptes scolaire» puis sur la liste déroulante droite 'Local'
* Entrez le chemin d’accès federationmetadata.xml pour votre déploiement d’ADFS et fournir un URI d’application (fournir tout URI pour l’instant, et vous allez le modifier ultérieurement), cliquez sur Ok pour ajouter le projet à la solution.

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* Cliquez avec le bouton droit sur les contrôleurs de dans l’Explorateur de solutions sous le nouveau projet créé. Sélectionnez Ajouter -> contrôleur
* Dans la sélection du modèle, sélectionnez «API Web 2 contrôleur - vide» et cliquez sur Ok.

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* Nommez le contrôleur approprié

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* Ajoutez le code suivant dans le contrôleur


        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Http;
        namespace WebAPIOBO.Controllers
        {
            public class WebAPIOBOController : ApiController
            {
                public IHttpActionResult Get()
                {
                    return Ok("WebAPI via OBO");
                }
            }
        }

Ce code retourne simplement la chaîne lorsque tout le monde place une demande Get pour le WebAPI WebAPIOBO

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>Ajout de la nouvel principal WebAPI à ADFS

Ouvrez le groupe d’applications MySampleGroup. Cliquez sur Ajouter l’application et sélectionner un modèle API Web et cliquez sur Suivant.

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

Sur la page Configurer les API Web fournissent un nom de l’entrée WebAPI et l’identificateur approprié. L’identificateur doit être la valeur URL SSL WebAPIOBO projet dans visual studio (semblable à ce que nous l’avons fait pour BackendWebAPIAdfsAdd).

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

Poursuivez le reste de l’Assistant même en tant que lorsque nous avons configuré le ToDoListService WebAPI. À la fin, votre groupe d’applications doit se présenter comme ci-dessous:

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>Modification du code ToDoListService

#### <a name="modifying-the-application-config"></a>Modification de la configuration de l’application

* Ouvrez le fichier Web.config
* Modifiez les clés suivantes

| Clé | Valeur |
|:-----|:-------|
|IDA: public| ID de la ToDoListService comme donné à ADFS lors de la configuration de la ToDoListService WebAPI, par exemple, https://localhost:44321/|
|IDA: ClientID| ID de la ToDoListService comme donné à ADFS lors de la configuration de la ToDoListService WebAPI, par exemple, https://localhost:44321/ </br>**Il est très important que l’ida: public et ida: ClientID correspondent aux autres**|
|IDA: ClientSecret| Il s’agit de la clé secrète qui ADFS généré lorsque vous ont été configuration du client de ToDoListService dans ADFS|
|IDA: ADFSMetadata| Il s’agit de l’URL pour vos métadonnées ADFS, pour par exemple, https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml|
|IDA: OBOWebAPIBase| Il s’agit de l’adresse de base que nous utiliserons pour appeler le API, principal pour par exemple, https://localhost:44300|
|IDA: autorité| Il s’agit de l’URL de votre service ADFS, exemple https://fs.anandmsft.com/adfs/|


Les clés de tous les autres ida: XXXXXXX dans le **appsettings** nœud peut être commenté ou supprimé

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>Modifier l’authentification à partir d’Azure AD pour ADFS

* Ouvrez le fichier Startup.Auth.cs
* Supprimez le code suivant

        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                TokenValidationParameters = new TokenValidationParameters{ SaveSigninToken = true }
            });

avec

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

#### <a name="modifying-the-todolistcontroller"></a>Modification de la ToDoListController

Ajoutez la référence à System.Web. Extensions. Modifier les membres de la classe en remplaçant le code ci-dessous

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The App Key is a credential used by the application to authenticate to Azure AD.
    // The Tenant is the name of the Azure AD tenant in which this application is registered.
    // The AAD Instance is the instance of Azure, for example public Azure or Azure China.
    // The Authority is the sign-in URL of the tenant.
    //
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string appKey = ConfigurationManager.AppSettings["ida:AppKey"];

    //
    // To authenticate to the Graph API, the app needs to know the Grah API's App ID URI.
    // To contact the Me endpoint on the Graph API we need the URL as well.
    //
    private static string graphResourceId = ConfigurationManager.AppSettings["ida:GraphResourceId"];
    private static string graphUserUrl = ConfigurationManager.AppSettings["ida:GraphUserUrl"];
    private const string TenantIdClaimType = "https://schemas.microsoft.com/identity/claims/tenantid";

avec

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The client secret is the credentials for the WebServer Client

    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

    // Base address of the WebAPI
    private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];

**Modifier la revendication utilisée pour le nom**

Nous sommes émission de la revendication Nmae depuis ADFS, mais nous ne sommes pas émission NameIdentifier revendication. L’exemple utilise NameIdentifier pour la clé de manière unique dans les éléments ToDo. Par souci de simplicité, vous pouvez supprimer en toute sécurité la NameIdentifier avec une revendication de nom dans le code. Rechercher et remplacer toutes les occurrences de NameIdentifier par nom.

**Modifier la routine Post et CallGraphAPIOnBehalfOfUser()**

Copiez et collez le code ci-dessous dans ToDoListController.cs et remplacez le code pour valider et CallGraphAPIOnBehalfOfUser

    // POST api/todolist
    public async Task Post(TodoItem todo)
    {
        if (!ClaimsPrincipal.Current.FindFirst("https://schemas.microsoft.com/identity/claims/scope").Value.Contains("user_impersonation"))
        {
            throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found" });
        }

        //
        // Call the WebAPIOBO On Behalf Of the user who called the To Do list web API.
        //
        string augmentedTitle = null;
        string custommessage = await CallGraphAPIOnBehalfOfUser();
        if (custommessage != null)
        {
            augmentedTitle = String.Format("{0}, Message: {1}", todo.Title, custommessage);
        }
        else
        {
            augmentedTitle = todo.Title;
        }

        if (null != todo && !string.IsNullOrWhiteSpace(todo.Title))
        {
            db.TodoItems.Add(new TodoItem { Title = augmentedTitle, Owner = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value });
            db.SaveChanges();
        }
    }

    public static async Task<string> CallGraphAPIOnBehalfOfUser()
    {
        string accessToken = null;
        AuthenticationResult result = null;
        AuthenticationContext authContext = null;
        HttpClient httpClient = new HttpClient();
        string custommessage = "";

        //
        // Use ADAL to get a token On Behalf Of the current user.  To do this we will need:
        //      The Resource ID of the service we want to call.
        //      The current user's access token, from the current request's authorization header.
        //      The credentials of this application.
        //      The username (UPN or email) of the user calling the API
        //
        ClientCredential clientCred = new ClientCredential(clientId, clientSecret);
        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        string userName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn) != null ? ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value : ClaimsPrincipal.Current.FindFirst(ClaimTypes.Email).Value;
        string userAccessToken = bootstrapContext.Token;
        UserAssertion userAssertion = new UserAssertion(bootstrapContext.Token, "urn:ietf:params:oauth:grant-type:jwt-bearer", userName);

        string userId = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
        authContext = new AuthenticationContext(authority, false);

        // In the case of a transient error, retry once after 1 second, then abandon.
        // Retrying is optional.  It may be better, for your application, to return an error immediately to the user and have the user initiate the retry.
        bool retry = false;
        int retryCount = 0;

        do
        {
            retry = false;
            try
            {
                result = authContext.AcquireToken(OBOWebAPIBase, clientCred, userAssertion);
                accessToken = result.AccessToken;
            }
            catch (AdalException ex)
            {
                if (ex.ErrorCode == "temporarily_unavailable")
                {
                    // Transient error, OK to retry.
                    retry = true;
                    retryCount++;
                    Thread.Sleep(1000);
                }
            }
        } while ((retry == true) && (retryCount < 1));

        if (accessToken == null)
        {
            // An unexpected error occurred.
            return (null);
        }

        // Once the token has been returned by ADAL, add it to the http authorization header, before making the call to access the To Do list service.
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

        // Call the WebAPIOBO.
        HttpResponseMessage response = await httpClient.GetAsync(OBOWebAPIBase + "/api/WebAPIOBO");


        if (response.IsSuccessStatusCode)
        {
            // Read the response and databind to the GridView to display To Do items.
            string s = await response.Content.ReadAsStringAsync();
            JavaScriptSerializer serializer = new JavaScriptSerializer();
            custommessage = serializer.Deserialize<string>(s);
            return custommessage;
        }
        else
        {
            custommessage = "Unsuccessful OBO operation : " + response.ReasonPhrase;
        }
        // An unexpected error occurred calling the Graph API.  Return a null profile.
        return (null);
    }

## <a name="running-the-solution"></a>Exécution de la solution


Par défaut, visual studio est configuré pour exécuter un projet lorsque vous appuyez sur le débogage à exécuter.

* Cliquez avec le bouton droit sur la solution et sélectionnez Propriétés.
* Dans la page de propriétés sélectionnez démarrage plusieurs projets et modifier l’Action de démarrage pour les trois entrées.

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

Appuyez sur F5 et exécuter la solution

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

Cliquez sur le bouton connexion. Vous devrez connectez-vous à l’aide d’ADFS

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

Après la connexion, ajoutez un élément ToDo dans la liste. En arrière-plan, nous allons effectuer une opération Post vers le ToDoListService qui fera davantage un billet sur le web WebAPIOBO API.

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

Vous verrez que l’élément a été ajouté à la liste avec le message supplémentaire à partir de l’API Web qui a accédé à l’aide d’auth OBO-flux principal sur le bon fonctionnement.

![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

Vous pouvez également afficher les traces détaillées sur Fiddler. Lancez Fiddler, puis activer le déchiffrement HTTPS. Vous pouvez voir que nous rendre deux demandes au point de terminaison /adfs/oautincludes.
Dans la première interaction, nous présenter le code d’accès au point de terminaison jeton et obtenir un accès jeton pour https://localhost:44321/ ![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

Dans la deuxième interaction avec le point de terminaison jeton, vous pouvez voir que nous avons **requested_token_use** définir en tant que **on_behalf_of** et nous utilisons le jeton d’accès obtenu pour le service web de couche intermédiaire, c'est-à-dire https://localhost:44321/ en tant que l’assertion pour obtenir le jeton de la part de.
![ADFS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>Étapes suivantes
[Développement d’ADFS](../../ad-fs/AD-FS-Development.md)  
