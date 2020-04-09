---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: Créer une application à plusieurs niveaux à l’aide de OBO (au nom de) à l’aide d’OAuth avec AD FS 2016 ou version ultérieure
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ed8bb6300360553e0809f4a30cec38bc37777ae9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858842"
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016-or-later"></a>Créer une application à plusieurs niveaux à l’aide de OBO (au nom de) à l’aide d’OAuth avec AD FS 2016 ou version ultérieure


Cette procédure pas à pas fournit des instructions pour l’implémentation d’une authentification OBO à l’aide de AD FS dans Windows Server 2016 TP5 ou version ultérieure. Pour en savoir plus sur l’authentification OBO, consultez [AD FS les flux OpenID Connect/OAuth et les scénarios d’application](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md) .

>AVERTISSEMENT : l’exemple que vous pouvez générer ici est fourni à titre éducatif uniquement. Ces instructions sont destinées à l’implémentation la plus simple et la plus minimale possible pour exposer les éléments requis du modèle. L’exemple peut ne pas inclure tous les aspects de la gestion des erreurs et d’autres fonctionnalités liées, et se concentre uniquement sur l’obtention d’une authentification OBO réussie.

## <a name="overview"></a>Overview

Dans cet exemple, nous allons créer un workflow d’authentification où un client accède à un service Web de niveau intermédiaire et le service Web agira ensuite pour le compte du client authentifié afin d’obtenir un jeton d’accès.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.png)

Voici le workflow d’authentification que l’exemple va atteindre
1. Le client s’authentifie auprès AD FS point de terminaison d’autorisation et demande un code d’autorisation
2. Le point de terminaison d’autorisation retourne le code d’authentification au client
3. Le client utilise le code d’authentification et le présente au point de terminaison de jeton AD FS pour demander un jeton d’accès pour le service Web de niveau intermédiaire en tant que WebAPI
4. AD FS retourne le jeton d’accès au service Web de niveau intermédiaire. Pour les fonctionnalités supplémentaires, le service de niveau intermédiaire a besoin d’accéder au serveur principal WebAPI
5. Le client utilise le jeton d’accès pour utiliser le service de niveau intermédiaire.
6. Le service de niveau intermédiaire fournit le jeton d’accès au point de terminaison du jeton AD FS et demande un jeton d’accès pour les WebAPI backend pour le compte de l’utilisateur authentifié
7. AD FS renvoie le jeton d’accès pour le WebAPI principal au service de niveau intermédiaire actiing en tant que client
8. Le service de niveau intermédiaire utilise le jeton d’accès fourni par AD FS à l’étape 7 pour accéder au serveur principal WebAPI en tant que client et exécuter les fonctions nécessaires

## <a name="sample-structure"></a>Exemple de structure

L’exemple se compose de trois modules


Module | Description
-------|------------
ToDoClient | Native Client avec lequel l’utilisateur interagit
ToDoService | API Web de niveau intermédiaire qui agit comme un client pour le serveur principal WebAPI
WebAPIOBO | API Web principale utilisée par ToDoService pour effectuer l’opération requise lorsque l’utilisateur ajoute un ToDoItem




## <a name="setting-up-the-development-box"></a>Configuration de la zone de développement

Cette procédure pas à pas utilise Visual Studio 2015. Le projet utilise fortement Bibliothèque d’authentification Active Directory (ADAL). Pour en savoir plus sur ADAL, consultez [bibliothèque d’Authentification Active Directory .net](https://msdn.microsoft.com/library/azure/mt417579.aspx)

L’exemple utilise également SQL Server, version 11.0. Installez la base de données locale SQL avant de travailler sur l’exemple.

## <a name="setting-up-the-environment"></a>Configuration de l'environnement
Nous allons travailler avec une configuration de base de :

1. **DC**: contrôleur de domaine pour le domaine dans lequel AD FS sera hébergé
2. **Serveur de AD FS**: serveur AD FS pour le domaine
3. **Ordinateur de développement**: ordinateur sur lequel Visual Studio est installé et qui développera notre exemple

Si vous le souhaitez, vous pouvez utiliser uniquement deux ordinateurs. Une pour DC/ADFS et l’autre pour le développement de l’exemple.

La configuration du contrôleur de domaine et du AD FS dépasse le cadre de cet article. Pour plus d’informations sur le déploiement, consultez :

- [Déploiement AD DS](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Déploiement d’AD FS](../AD-FS-Deployment.md)

L’exemple est basé sur l’exemple OBO existant sur Azure créé par Vittorio Bertocci et est disponible [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof). Suivez les instructions pour cloner le projet sur votre ordinateur de développement et créer une copie de l’exemple pour commencer à utiliser.

## <a name="clone-or-download-this-repository"></a>Cloner ou télécharger ce référentiel

À partir de votre shell ou de la ligne de commande :

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>Modification de l’exemple

Dès que vous ouvrez la solution WebAPI-OnBehalfOf-DotNet. sln, vous remarquerez que deux projets se trouvent dans la solution

* **ToDoListClient**: il s’agit du client OpenID avec lequel l’utilisateur interagit
* **ToDoListService**: il s’agit de l’application/service WebServer de niveau intermédiaire qui interagit avec un autre WEBAPI principal OBO l’utilisateur authentifié

Comme vous pouvez le voir, nous devons ajouter un autre projet ultérieurement, qui agira en tant que ressource accessible par le ToDoListService de niveau intermédiaire.

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>Configuration de AD FS pour l’application cliente et webserver

Dans le formulaire actuel de l’exemple, l’authentification est configurée pour être effectuée par rapport à Azure AD. Nous voulons modifier le mécanisme d’authentification et le diriger vers AD FS déployé localement. Pour ce faire, nous devons configurer AD FS pour reconnaître l’application cliente et le serveur WebServer que nous avons dans l’exemple.

**Création d’un groupe d’applications**

Ouvrez la console MMC de gestion des AD FS et ajoutez un nouveau groupe d’applications. Sélectionnez le modèle WebAPI-application-native.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

Cliquez sur suivant pour afficher la page permettant de fournir des informations sur l’application cliente. Donnez un nom approprié à l’application cliente dans AD FS. Copiez l’identificateur du client et enregistrez-le quelque part. vous pourrez y accéder ultérieurement, car il sera requis dans la configuration de l’application dans Visual Studio.

>Remarque : l’URI de redirection peut être n’importe quel URI arbitraire, car il n’est pas vraiment utilisé dans le cas de clients natifs

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

Cliquez sur suivant pour afficher la page permettant de fournir des informations sur WebAPI. Donnez un nom approprié à l’entrée AD FS pour le WebAPI et entrez l’URI de redirection comme URI que vous voyez dans Visual Studio pour le ToDoListService

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

Cliquez sur suivant pour afficher la page choisir une stratégie de Access Control. Vérifiez que vous voyez « autoriser tout le monde » dans la section stratégie.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

Cliquez sur suivant pour afficher la page Configurer les autorisations de l’application. Dans cette page, sélectionnez les étendues autorisées en tant que OpenID (sélectionnées par défaut) et user_impersonation. L’étendue « user_impersonation » est nécessaire pour pouvoir demander un jeton d’accès de la part de AD FS.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

Cliquez sur suivant pour afficher la page Résumé. Parcourez le reste de l’Assistant et terminez la configuration.

Pour permettre l’authentification pour le compte de, nous devons nous assurer que AD FS retourne un jeton d’accès avec une étendue user_impersonation au client. Modifiez l’émission des revendications pour ToDoListServiceWebApi pour inclure les trois règles personnalisées suivantes :

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "http://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**Ajout de ToDoListService en tant que client dans le groupe d’applications**

À ce niveau, nous devons effectuer une entrée supplémentaire dans AD FS pour que l’application WebServer agisse comme un client et non comme une ressource. Ouvrez le groupe d’applications que vous venez de créer, puis cliquez sur Ajouter une application.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

La page « Ajouter une nouvelle application à MySampleGroup » s’affiche. Sur cette page, sélectionnez « application serveur ou site Web » comme application autonome.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

Cliquez sur suivant pour afficher la page pour fournir des détails sur l’application. Fournissez un nom approprié pour l’entrée de configuration dans la section nom. Vérifiez que l’identificateur du client est identique à l’identificateur du ToDoListServiceWebAPI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

Cliquez sur suivant pour afficher la page permettant de configurer les informations d’identification de l’application. Cliquez sur « générer un secret partagé ». Un secret qui est généré automatiquement s’affiche. Copiez la clé secrète à un endroit où cela sera nécessaire lors de la configuration de ToDoListService dans Visual Studio.


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

Cliquez sur suivant et terminez l’Assistant.

### <a name="modifying-the-todolistclient-code"></a>Modification du code ToDoListClient

#### <a name="modify-the-application-config"></a>Modifier la configuration de l’application

Accédez à votre projet ToDoListClient dans WebAPI-OnBehalfOf-DotNet solution. Ouvrez le fichier app. config et apportez les modifications suivantes

* Commenter l’entrée Ida : locataire Key
* Pour Ida : RedirectURI, entrez l’URI arbitraire que vous avez fourni lors de la configuration de l’MySampleGroup_ClientApplication dans AD FS.
* Pour la clé Ida : ClientID, fournissez l’identificateur d’ID client que AD FS a donné lors de la configuration du MySampleGroup_ClientApplication.
* Pour Ida : ToDoListResourceID, fournissez l’ID de ressource que vous avez donné lors de la configuration du ToDoListServiceWebApi dans AD FS
* Commentez la clé Ida : AADInstance
* Pour Ida : ToDoListBaseAddress, entrez l’ID de ressource du ToDoListServiceWebApi. Ce sera utilisé lors de l’appel de ToDoList WebAPI.
* Ajoutez une clé Ida : Authority et fournissez la valeur en tant qu’URI pour AD FS.

Votre **appSettings** dans App. config doit ressembler à ceci :

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

Commenter la ligne qui lit les informations du locataire à partir de la configuration de l’application

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

Remplacez la valeur de String Authority par

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

Modifiez le code pour lire les valeurs correctes de ToDoListResourceId et ToDoListBaseAddress

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

Dans la fonction MainWindow (), modifiez l’initialisation authcontext comme suit :

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>Ajout de la ressource principale

Pour exécuter le processus de la part de, vous devez créer une ressource backend à laquelle l’ToDoListService sera accessible au nom de l’utilisateur authentifié... Le choix de la ressource principale peut varier en fonction de l’exigence, mais pour les besoins de cet exemple, vous pouvez créer un WebAPI de base.

* Cliquez avec le bouton droit sur la solution’WebAPI-OnBehalfOf-DotNet’dans l’Explorateur de solutions, puis sélectionnez Ajouter-> nouveau projet.
* Choisir le modèle d’application Web ASP.NET

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* À l’invite suivante, cliquez sur « modifier l’authentification »
* Sélectionnez « comptes professionnels et scolaires » et dans la liste déroulante de droite, sélectionnez « local ».
* Entrez le chemin d’accès FederationMetadata. xml pour votre déploiement AD FS et fournissez un URI d’application (fournissez un URI pour le moment, et modifiez-le ultérieurement), puis cliquez sur OK pour ajouter le projet à la solution.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* Cliquez avec le bouton droit sur Controllers dans l’Explorateur de solutions sous le nouveau projet créé. Sélectionner le contrôleur Add->
* Dans la sélection du modèle, sélectionnez « contrôleur Web API 2-vide », puis cliquez sur OK.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* Attribuez un nom approprié au contrôleur.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* Ajoutez le code suivant dans le contrôleur :

```cs
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http;
    namespace WebAPIOBO.Controllers
    {
        [Authorize]
        public class WebAPIOBOController : ApiController
        {
            public IHttpActionResult Get()
            {
                return Ok($"WebAPI via OBO (user: {User.Identity.Name}");
            }
        }
    }
```

Ce code renverra simplement la chaîne quand quelqu’un place une requête d’extraction pour le WebAPI WebAPIOBO

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>Ajout du nouveau WebAPI backend à AD FS

Ouvrez le groupe d’applications MySampleGroup. Cliquez sur Ajouter une application et sélectionnez modèle d’API Web, puis cliquez sur suivant.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

Dans la page Configurer l’API Web, fournissez un nom approprié pour l’entrée WebAPI et l’identificateur. L’identificateur doit être l’URL SSL de valeur du projet WebAPIOBO dans Visual Studio (similaire à ce que nous avons fait pour BackendWebAPIAdfsAdd).

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

Poursuivez le reste de l’Assistant comme lorsque nous avons configuré le WebAPI ToDoListService. À la fin, votre groupe d’applications doit ressembler à ceci :

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>Modification du code ToDoListService

#### <a name="modifying-the-application-config"></a>Modification de la configuration de l’application

* Ouvrir le fichier Web. config
* Modifiez les clés suivantes

| Clé                      | Valeur                                                                                                                                                                                                                   |
|:-------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ida : audience             | ID du ToDoListService donné à AD FS lors de la configuration du WebAPI ToDoListService, par exemple, https://localhost:44321/                                                                                         |
| Ida : ClientID             | ID du ToDoListService donné à AD FS lors de la configuration du WebAPI ToDoListService, par exemple, <https://localhost:44321/> </br>**Il est très important que Ida : audience et Ida : ClientID correspondent** |
| Ida : ClientSecret         | Il s’agit de la clé secrète que AD FS générée lorsque vous configurez le client ToDoListService dans AD FS                                                                                                                   |
| Ida : AdfsMetadataEndpoint | Il s’agit de l’URL de vos métadonnées de AD FS, par exemple https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml                                                                                             |
| Ida : OBOWebAPIBase        | Il s’agit de l’adresse de base que nous allons utiliser pour appeler l’API backend, par exemple https://localhost:44300                                                                                                                     |
| Ida : autorité            | Il s’agit de l’URL de votre service AD FS, par exemple https://fs.anandmsft.com/adfs/                                                                                                                                          |

Toutes les autres clés Ida : XXXXXXx du nœud **appSettings** peuvent être commentées ou supprimées

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>Modifiez l’authentification de Azure AD en AD FS

* Ouvrez le fichier Startup.Auth.cs
* Supprimez le code suivant

        app.UseWindowsAzureActiveDirectoryBearerAuthentication(
            new WindowsAzureActiveDirectoryBearerAuthenticationOptions
            {
                Audience = ConfigurationManager.AppSettings["ida:Audience"],
                Tenant = ConfigurationManager.AppSettings["ida:Tenant"],
                TokenValidationParameters = new TokenValidationParameters{ SaveSigninToken = true }
            });

par

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

#### <a name="modifying-the-todolistcontroller"></a>Modification du ToDoListController

Ajoutez une référence à System. Web. extensions. Modifiez les membres de classe en remplaçant le code ci-dessous

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

par

    //
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    // The client secret is the credentials for the WebServer Client

    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

    // Base address of the WebAPI
    private static string OBOWebAPIBase = ConfigurationManager.AppSettings["ida:OBOWebAPIBase"];

**Modifier la revendication utilisée pour le nom**

À partir de AD FS nous émettant la revendication NMAE, mais nous n’avons pas émis de revendication NameIdentifier. L’exemple utilise NameIdentifier pour une clé unique dans les éléments ToDo. Pour plus de simplicité, vous pouvez supprimer en toute sécurité le NameIdentifier avec une revendication de nom dans le code. Recherchez et remplacez toutes les occurrences de NameIdentifier par nom.

**Modifier la routine de publication et CallGraphAPIOnBehalfOfUser ()**

Copiez et collez le code ci-dessous dans ToDoListController.cs et remplacez le code de la publication et CallGraphAPIOnBehalfOfUser

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
        // The Resource ID of the service we want to call.
        // The current user's access token, from the current request's authorization header.
        // The credentials of this application.
        // The username (UPN or email) of the user calling the API
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
                    result = await authContext.AcquireTokenAsync(OBOWebAPIBase, clientCred, userAssertion);
                    //result = await authContext.AcquireTokenAsync(...);
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


Par défaut, Visual Studio est configuré pour exécuter un projet lorsque vous atteignez Debug pour l’exécuter.

* Cliquez avec le bouton droit sur la solution et sélectionnez Propriétés.
* Dans la page Propriétés, sélectionnez plusieurs projets de démarrage et modifiez l’action pour démarrer les trois entrées.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

Appuyez sur F5 et exécutez la solution

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

Cliquez sur le bouton de connexion. Vous serez invité à vous connecter à l’aide de AD FS

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

Après vous être connecté, ajoutez un élément ToDo dans la liste. En arrière-plan, nous allons effectuer une opération de publication sur le ToDoListService qui effectue une publication sur l’API Web WebAPIOBO.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

En cas de réussite de l’opération, vous verrez que l’élément a été ajouté à la liste avec le message supplémentaire de l’API Web principale accessible à l’aide de OBO auth-Flow.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

Vous pouvez également consulter les suivis détaillés sur Fiddler. Lancez Fiddler et activez le déchiffrement HTTPs. Vous pouvez voir que nous effectuons deux demandes sur le point de terminaison/ADFS/oautincludes.
Dans la première interaction, nous présentons le code d’accès au point de terminaison de jeton et obtenons un jeton d’accès pour https://localhost:44321/ ![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

Dans la deuxième interaction avec le point de terminaison de jeton, vous pouvez voir que nous avons **requested_token_use** défini comme **on_behalf_of** et que nous utilisons le jeton d’accès obtenu pour le service Web de niveau intermédiaire, c’est-à-dire https://localhost:44321/ comme assertion pour obtenir le jeton pour le compte de.
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>Étapes suivantes
[Développement des services AD FS](../../ad-fs/AD-FS-Development.md)  
