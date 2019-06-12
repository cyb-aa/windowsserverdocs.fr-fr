---
ms.assetid: 5052f13c-ff35-471d-bff5-00b5dd24f8aa
title: Créer une application à plusieurs niveaux à l’aide pour le compte (OBO) à l’aide d’OAuth avec AD FS 2016 ou version ultérieure
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 047f297cfaabff3cbbd45057a4198e2fd2e747de
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445450"
---
# <a name="build-a-multi-tiered-application-using-on-behalf-of-obo-using-oauth-with-ad-fs-2016-or-later"></a>Créer une application à plusieurs niveaux à l’aide pour le compte (OBO) à l’aide d’OAuth avec AD FS 2016 ou version ultérieure


Cette procédure pas à pas fournit des instructions pour l’implémentation d’une on-behalf-of (OBO) d’authentification à l’aide d’AD FS dans Windows Server 2016 TP5 ou version ultérieure. Pour en savoir plus sur l’authentification OBO lisez [scénarios AD FS pour les développeurs](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)

>AVERTISSEMENT : L’exemple que vous pouvez générer ici est à titre éducatif uniquement. Ces instructions concernent l’implémentation la plus simple, plus minimale possible d’exposer les éléments requis du modèle. L’exemple ne peut pas inclure tous les aspects de la gestion des erreurs et d’autres sont liées à des fonctionnalités et se concentre uniquement sur bien une authentification OBO réussie.

## <a name="overview"></a>Vue d'ensemble

Dans cet exemple, nous allons créer un flux d’authentification dans lequel un client accéderont à un Service Web de couche intermédiaire et le service web sera ensuite agir au nom du client pour obtenir un jeton d’accès authentifié.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO28.png)

Voici le flux d’authentification qui permet d’obtenir l’exemple
1. Client s’authentifie auprès d’un point de terminaison d’autorisation AD FS et qu’il demande un code d’autorisation
2. Point de terminaison d’autorisation renvoie le code d’authentification client
3. Utilise le code d’authentification client et les présente au point de terminaison jeton AD FS pour demander un jeton accès pour intermédiaire niveau de Service Web, API Web
4. AD FS renvoie le jeton d’accès au Service Web de niveau intermédiaire. Pour des fonctionnalités supplémentaires, Service de niveau intermédiaire nécessite un accès à la Backend WebAPI
5. Client utilise le jeton d’accès pour utiliser le service de niveau intermédiaire.
6. Service de niveau intermédiaire fournit le jeton d’accès pour le point de terminaison de jeton AD FS et demandes de jeton d’accès pour Backend WebAPI sur la part de l’utilisateur authentifié
7. AD FS renvoie le jeton d’accès pour le serveur principal WebAPI pour actiing de Service de niveau intermédiaire en tant que client
8. Service de niveau intermédiaire utilise le jeton d’accès fourni par AD FS à l’étape 7 pour accéder au serveur principal WebAPI en tant que client et exécute des fonctions nécessaires

## <a name="sample-structure"></a>Exemple de Structure

Exemple constitueront de trois modules


Module | Description
-------|------------
ToDoClient | Client natif avec lequel l’utilisateur interagit
ToDoService | Intermédiaire couche API web qui agit comme un client pour le serveur principal WebAPI
WebAPIOBO | Back-end web api qui est utilisé par ToDoService pour effectuer l’opération requise lorsque l’utilisateur ajoute un élément de tâche




## <a name="setting-up-the-development-box"></a>Configuration de la boîte de développement

Cette procédure pas à pas utilise Visual Studio 2015. Le projet utilise massivement Active Directory Authentication Library (ADAL). Pour en savoir plus sur la bibliothèque ADAL, consultez [.NET de bibliothèque d’authentification Active Directory](https://msdn.microsoft.com/library/azure/mt417579.aspx)

L’exemple utilise également la base de données SQL locale v11.0. Installer la base de données SQL locale avant de travailler sur l’exemple.

## <a name="setting-up-the-environment"></a>Configuration de l’environnement
Nous travaillons avec une configuration de base de :

1. **CONTRÔLEUR DE DOMAINE**: Contrôleur de domaine pour le domaine dans lequel les services AD FS sera hébergée
2. **Serveur AD FS**: Le serveur AD FS pour le domaine
3. **Ordinateur de développement**: Ordinateur sur lequel nous Visual Studio est installé et que vous développerez notre exemple

Vous pouvez utiliser si vous le souhaitez, seuls deux machines. Une pour le contrôleur de domaine/AD FS et l’autre pour le développement de l’exemple.

Comment configurer le contrôleur de domaine et d’AD FS n’entre pas dans la portée de cet article. Pour des informations supplémentaires sur le déploiement, consultez :

- [Déploiement AD DS](../../ad-ds/deploy/AD-DS-Deployment.md)
- [Déploiement d’AD FS](../AD-FS-Deployment.md)

L’exemple se base sur l’exemple existant de OBO auprès d’Azure créé par Vittorio Bertocci et disponible [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof). Suivez les instructions pour cloner le projet sur votre ordinateur de développement et de créer une copie de l’exemple pour commencer à travailler avec.

## <a name="clone-or-download-this-repository"></a>Clonez ou téléchargez ce dépôt

À partir de votre interpréteur de commandes ou de la ligne de commande :

    git clone https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof.git

## <a name="modifying-the-sample"></a>Modification de l’exemple

Dès que vous ouvrez la solution WebAPI-OnBehalfOf-DotNet.sln, vous remarquerez que vous avez deux projets dans la solution

* **ToDoListClient**: Il servira au client OpenID qui d’interagir avec l’utilisateur
* **ToDoListService**: Cela sera être l’application de serveur Web de niveau intermédiaire / de Service que d’interagir avec un autre serveur principal WebAPI OBO l’utilisateur authentifié

Comme vous pouvez le voir, nous devrons ajouter un autre projet ultérieurement qui se comporte comme la ressource qui sera accessible par ToDoListService de couche intermédiaire.

### <a name="configuring-ad-fs-for-the-client-and-webserver-app"></a>Configuration d’AD FS pour le Client et l’application de serveur Web

Dans l’écran actuel de l’exemple, l’authentification est configurée pour être effectuées dans Azure AD. Nous voulons modifier le mécanisme d’authentification et direct il vers AD FS déployé en local. Pour ce faire, nous devons configurer AD FS pour reconnaître le client et serveur Web application nous dans l’exemple.

**Création d’un groupe d’applications**

Ouvrez la console MMC de gestion AD FS et ajouter un nouveau groupe d’applications. Sélectionner un modèle natif-Application-API Web.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO2.PNG)

Cliquez sur Suivant et s’affiche avec la page pour fournir des informations sur l’application cliente. Donnez un nom approprié au client application dans AD FS. Copier l’identificateur de client et l’enregistrer dans un endroit que vous pourrez accéder ultérieurement comme cela sera nécessaire dans la configuration de l’application dans visual studio.

>Remarque: L’URI de redirection peut être n’importe quel URI arbitraire car elle ne sert pas vraiment dans le cas des clients natifs

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO11.PNG)

Cliquez sur Suivant et s’affiche avec la page pour fournir des informations sur les API Web. Donnez un nom approprié pour l’entrée d’AD FS pour l’API Web et entrez l’URI de redirection en tant que l’URI que vous voyez dans Visual Studio pour ToDoListService

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO16.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO18.PNG)

Cliquez sur Suivant et vous verrez la Page Choisir la stratégie de contrôle d’accès. Vérifiez que vous voyez « Autoriser tout le monde » dans la section de stratégie.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO1.PNG)

Cliquez sur Suivant et s’affiche avec la page Configurer les autorisations d’Application. Dans cette page, sélectionnez les étendues autorisées en tant qu’openid (sélectionnée par défaut) et user_impersonation. L’étendue « user_impersonation » est nécessaire pour pouvoir correctement demander un jeton d’accès on-behalf-of d’AD FS.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO12.PNG)

Cliquez sur suivant affiche la page de résumé. Passez en revue le reste de l’Assistant et terminer la configuration.

Pour activer l’authentification on-behalf-of, nous avons besoin pour vous assurer que AD FS renvoie un jeton d’accès avec l’étendue user_impersonation au client. Modifiez l’émission de revendications pour ToDoListServiceWebApi inclure les trois règles personnalisées suivantes :

    @RuleName = "All claims"
    c:[]
    => issue(claim = c);

    @RuleName = "Issue user_impersonation scope"
    => issue(Type = "https://schemas.microsoft.com/identity/claims/scope", Value = "user_impersonation");

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO10.PNG)

**Ajout de ToDoListService en tant que client dans le groupe d’applications**

À ce stade, nous devons créer une entrée supplémentaire dans AD FS pour l’application de serveur Web d’agir en tant que client et pas seulement en tant que ressource. Ouvrez le groupe d’applications que vous venez de créer, puis cliquez sur Ajouter une Application.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO15.PNG)

La page « Ajouter une nouvelle application à MySampleGroup » s’affiche. Dans cette page, sélectionnez « Application ou site Web Server » en tant que l’application autonome

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO19.PNG)

Cliquez sur Suivant et s’affiche avec la page pour fournir des détails de l’application. Indiquez un nom approprié pour l’entrée de configuration dans la section de nom. Vérifiez que l’identificateur de Client est identique à l’identificateur pour le ToDoListServiceWebAPI

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO20.PNG)

Cliquez sur Suivant et s’affiche avec la page pour configurer les informations d’identification de l’application. Cliquez sur « Générer un secret partagé ». S’affiche avec une clé secrète qui est générée automatiquement. Copiez la clé secrète au même emplacement, car cela sera nécessaire pendant que nous allons configurer ToDoListService dans visual studio.


![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO17.PNG)

Cliquez sur Suivant et terminez l’Assistant.

### <a name="modifying-the-todolistclient-code"></a>Modification du code ToDoListClient

#### <a name="modify-the-application-config"></a>Modifier la configuration de l’Application

Accédez à vous êtes le projet ToDoListClient dans OnBehalfOf-WebAPI-DotNet solution. Ouvrez le fichier App.config et apportez les modifications suivantes

* Commentaire de l’entrée de clé ida : client
* Pour l’ida : URI de redirection, entrez l’URI arbitraire que vous avez fourni lors de la configuration de la MySampleGroup_ClientApplication dans AD FS.
* Pour la clé ida : ClientID, fournir au client identifiant AD FS a donné lors de la configuration de la MySampleGroup_ClientApplication.
* Pour l’ida : ToDoListResourceID fournissez l’ID de ressource que vous avez donné lors de la configuration de la ToDoListServiceWebApi dans AD FS
* Commentaire de la clé ida : AADInstance
* Pour l’ida : ToDoListBaseAddress, entrez l’ID de ressource de la ToDoListServiceWebApi. Il sera utilisé lors de l’appel de la ToDoList WebAPI.
* Ajouter une clé ida : autorité et fournir la valeur comme URI pour AD FS.

Votre **appSettings** dans le fichier App.Config doit ressembler à ceci :

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

La lecture des informations de locataire à partir de la configuration de l’application de ligne de commentaire

    //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];

Modifiez la valeur de l’autorité de chaîne pour

    private static string authority = ConfigurationManager.AppSettings["ida:Authority"];

Modifier le code pour lire les valeurs correctes de ToDoListResourceId et ToDoListBaseAddress

    private static string todoListResourceId = ConfigurationManager.AppSettings["ida:TodoListResourceId"];
    private static string todoListBaseAddress = ConfigurationManager.AppSettings["ida:TodoListBaseAddress"];

Dans la fonction MainWindow() modifier l’initialisation authcontext en tant que :

    authContext = new AuthenticationContext(authority, false);

### <a name="adding-the-backend-resource"></a>Ajout de la ressource principale

Afin de terminer le flux on-behalf-of, vous devez créer une ressource principale qui accédera à ToDoListService sur à la place de l’utilisateur authentifié. Le choix de la ressource principale peut varier selon les besoins, mais pour les besoins de cet exemple, vous pouvez créer une API Web de base.

* Cliquez avec le bouton droit sur la solution « OnBehalfOf-WebAPI-DotNet » dans l’Explorateur de solutions et sélectionnez Ajouter -> Nouveau projet
* Choisissez le modèle Application Web ASP.NET

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO4.PNG)

* Sur la prochaine invite, cliquez sur « Modifier l’authentification »
* Sélectionnez « Comptes professionnels et scolaires » puis sur la liste déroulante droite 'Local'
* Entrez le chemin d’accès federationmetadata.xml pour votre déploiement AD FS et fournir un URI d’application (fournir n’importe quel URI pour le moment, et vous allez le modifier ultérieurement) et cliquez sur Ok pour ajouter le projet à la solution.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO9.PNG)

* Cliquez avec le bouton droit sur les contrôleurs dans l’Explorateur de solutions sous le nouveau projet créé. Sélectionnez Ajouter -> contrôleur
* Dans la sélection du modèle, sélectionnez « Web API 2 Controller - vides, puis cliquez sur Ok.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO3.PNG)

* Nommez le contrôleur approprié

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO13.PNG)

* Ajoutez le code suivant dans le contrôleur


~~~
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
~~~

Ce code retourne simplement la chaîne lorsque tout le monde place une demande Get pour la WebAPI WebAPIOBO

### <a name="adding-the-new-backend-webapi-to-ad-fs"></a>Ajouter le nouveau serveur principal WebAPI à AD FS

Ouvrez le groupe d’applications MySampleGroup. Cliquez sur Ajouter une application et sélectionner un modèle API Web et cliquez sur Suivant.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO6.PNG)

Sur la page Configurer les API Web fournissent un nom approprié pour l’entrée de l’API Web et l’identificateur. L’identificateur doit être la valeur URL SSL WebAPIOBO projet dans visual studio (semblable à ce que nous avons fait pour BackendWebAPIAdfsAdd).

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO8.PNG)

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO7.PNG)

Continuez jusqu'à la fin de l’Assistant même en tant que lorsque nous avons configuré la ToDoListService WebAPI. À la fin, le groupe de votre application doit ressembler à ceci :

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO5.PNG)


### <a name="modifying-the-todolistservice-code"></a>Modification du code ToDoListService

#### <a name="modifying-the-application-config"></a>Modification de la configuration de l’application

* Ouvrez le fichier Web.config
* Modifier les clés suivantes

| Touche                      | Value                                                                                                                                                                                                                   |
|:-------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ida:Audience             | ID de ToDoListService selon les indications à AD FS lors de la configuration de la ToDoListService WebAPI, par exemple, https://localhost:44321/                                                                                         |
| ida:ClientID             | ID de ToDoListService selon les indications à AD FS lors de la configuration de la ToDoListService WebAPI, par exemple, <https://localhost:44321/> </br>**Il est très important que l’ida : public et ida : ClientID correspondent aux uns des autres** |
| ida:ClientSecret         | Il s’agit du secret AD FS généré lorsque vous avez configuré le client ToDoListService dans AD FS                                                                                                                   |
| ida:AdfsMetadataEndpoint | Cela concerne l’URL pour vos métadonnées ADFS, par exemple https://fs.anandmsft.com/federationmetadata/2007-06/federationmetadata.xml                                                                                             |
| ida:OBOWebAPIBase        | Ceci est l’adresse de base que nous utiliserons pour appeler l’API serveur principal, pour, par exemple https://localhost:44300                                                                                                                     |
| ida:Authority            | Ceci est l’URL de votre service AD FS, exemple https://fs.anandmsft.com/adfs/                                                                                                                                          |

Les clés de toutes les autres ida : XXXXXXX dans le **appsettings** nœud peut être commenté ou supprimé

#### <a name="change-authentication-from-azure-ad-to-ad-fs"></a>Modifier l’authentification à partir d’Azure AD et AD FS

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

#### <a name="modifying-the-todolistcontroller"></a>Modification de la ToDoListController

Ajoutez la référence à System.Web.Extensions. Modifier les membres de classe en remplaçant le code ci-dessous

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

À partir d’AD FS, nous sommes émettre la revendication Nmae, mais nous n’avons pas émettant revendication NameIdentifier. L’exemple utilise NameIdentifier à identifie de façon unique la clé dans les éléments de tâche. Par souci de simplicité, vous pouvez supprimer en toute sécurité le NameIdentifier avec la revendication de nom dans le code. Recherchez et remplacez toutes les occurrences de NameIdentifier par nom.

**Modifier la routine de Post et CallGraphAPIOnBehalfOfUser()**

Copiez et collez le code ci-dessous dans ToDoListController.cs et remplacez le code pour valider et CallGraphAPIOnBehalfOfUser

    // POST api/todolist
    public async Task Post(TodoItem todo)
    {
      if (!ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value.Contains("user_impersonation"))
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


Par défaut, visual studio est configuré pour exécuter un projet lorsque vous atteignez le débogage à exécuter.

* Clic avec le bouton droit sur la solution et sélectionnez Propriétés.
* Dans la page de propriétés sélectionnez des projets de démarrage multiples et modifiez l’Action de démarrage pour les trois entrées.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO14.PNG)

Appuyez sur F5 et exécutez la solution

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO24.PNG)

Cliquez sur le bouton de connexion. Vous êtes invité à se connecter en utilisant AD FS

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO25.PNG)

Après l’ouverture de session, ajoutez un élément ToDo dans la liste. Dans les coulisses, nous allons effectuer une opération Post vers ToDoListService qui effectuera plus un billet sur le web WebAPIOBO API.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO26.PNG)

Sur opération réussie, vous verrez que l’élément a été ajouté à la liste avec le message supplémentaire à partir de l’API Web qui a accédé à l’aide de flux d’authentification OBO principale.

![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO27.PNG)

Vous pouvez également voir les traces détaillées dans Fiddler. Lancer Fiddler et activer le déchiffrement HTTPS. Vous pouvez voir que nous envoyer des deux requêtes au point de terminaison /adfs/oautincludes.
Dans la première interaction, nous présenter le code d’accès au point de terminaison de jeton et obtenir un jeton d’accès https://localhost:44321/ ![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO22.PNG)

Dans la deuxième interaction avec le point de terminaison de jeton, vous pouvez voir que nous avons **requested_token_use** définir en tant que **on_behalf_of** et nous utilisons le jeton d’accès obtenu pour le service web de niveau intermédiaire, par exemple, https://localhost:44321/ en tant que l’assertion pour obtenir le jeton on-behalf-of.
![AD FS OBO](media/AD-FS-On-behalf-of-Authentication-in-Windows-Server-2016/ADFS_OBO23.PNG)

## <a name="next-steps"></a>Étapes suivantes
[Développement des services AD FS](../../ad-fs/AD-FS-Development.md)  
