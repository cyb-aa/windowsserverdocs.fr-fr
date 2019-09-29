---
title: Personnaliser les revendications à émettre dans id_token lors de l’utilisation de OpenID Connect ou OAuth avec AD FS 2016 ou version ultérieure
description: Présentation technique des concepts de jeton d’ID personnalisés dans AD FS 2016 ou version ultérieure
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: 88ae6837872c5a6cf6bb1d8533a0aa14b82ca573
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358908"
---
# <a name="customize-claims-to-be-emitted-in-id_token-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>Personnaliser les revendications à émettre dans id_token lors de l’utilisation de OpenID Connect ou OAuth avec AD FS 2016 ou version ultérieure

## <a name="overview"></a>Vue d'ensemble
[Cet article montre comment](native-client-with-ad-fs.md) générer une application qui utilise AD FS pour la connexion OpenID Connect. Toutefois, par défaut, il n’existe qu’un ensemble fixe de revendications disponibles dans le id_token. Les versions 2016 et ultérieures de AD FS peuvent personnaliser les id_token dans les scénarios OpenID Connect.

## <a name="when-are-custom-id-token-used"></a>Quand le jeton d’ID personnalisé est-il utilisé ?
Dans certains scénarios, il est possible que l’application cliente ne dispose pas d’une ressource à laquelle elle tente d’accéder. Par conséquent, il n’a pas vraiment besoin d’un jeton d’accès. Dans ce cas, l’application cliente a uniquement besoin d’un jeton d’ID, mais avec des revendications supplémentaires pour aider à la fonctionnalité.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>Quelles sont les restrictions relatives à l’obtention de revendications personnalisées dans le jeton d’ID ?

### <a name="scenario-1"></a>Scénario 1

![identifier](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode est défini en tant que form_post
2.  Seuls les clients publics peuvent recevoir des revendications personnalisées dans le jeton d’ID
3.  L’identificateur de la partie de confiance (identificateur de l’API Web) doit être identique à l’identificateur du client

### <a name="scenario-2"></a>Scénario 2

![identifier](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Avec [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) installé sur vos serveurs AD FS
1.  response_mode est défini en tant que form_post
2.  Les clients publics et confidentiels peuvent recevoir des revendications personnalisées dans le jeton d’ID
3.  Assignez l’étendue allatclaims à la paire client-RP.
Vous pouvez assigner l’étendue à l’aide de l’applet de commande Grant-ADFSApplicationPermission comme indiqué dans l’exemple ci-dessous :

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Création et configuration d’une application OAuth pour gérer les revendications personnalisées dans le jeton d’ID
Suivez les étapes ci-dessous pour créer et configurer l’application dans AD FS pour la réception d’un jeton d’ID avec des revendications personnalisées.

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>Créer et configurer un groupe d’applications dans AD FS 2016 ou version ultérieure

1. Dans AD FS gestion, cliquez avec le bouton droit sur groupes d’applications, puis sélectionnez **Ajouter un groupe d’applications**.

2. Dans l’Assistant groupe d’applications, pour le nom, entrez **ADFSSSO** , puis sous applications client-serveur, sélectionnez l' **application native qui accède à un modèle d’application Web** . Cliquez sur **Suivant**.

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. Copiez la valeur de l' **identificateur du client** .  Il sera utilisé ultérieurement comme valeur pour Ida : ClientId dans le fichier Web. config des applications.

4. Entrez les informations suivantes pour l' **URI de redirection :**  - . **https://localhost:44320/**  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. Dans l’écran **configurer l’API Web** , entrez les informations - suivantes pour l' **https://contoso.com/WebApp** identificateur.  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.  Cette valeur sera utilisée ultérieurement pour **Ida : ResourceId** dans le fichier Web. config des applications.

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. Dans l’écran **choisir une stratégie de Access Control** , sélectionnez **autoriser tout le monde** , puis cliquez sur **suivant**.

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. Dans l’écran **configurer les autorisations** de l’application, assurez-vous que **OpenID** et **allatclaims** sont sélectionnés, puis cliquez sur **suivant**.

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.png)

8. Dans l’écran **Résumé** , cliquez sur **suivant**.  

   ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.png)

9. Dans l’écran **terminé** , cliquez sur **Fermer**.

10. Dans AD FS gestion, cliquez sur groupes d’applications pour afficher la liste de tous les groupes d’applications. Cliquez avec le bouton droit sur **ADFSSSO** et sélectionnez **Propriétés**. Sélectionnez **ADFSSSO-API Web** , puis cliquez sur **modifier...**

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.png)

11. Dans l’écran **ADFSSSO-propriétés de l’API Web** , sélectionnez l’onglet **règles de transformation d’émission** , puis cliquez sur Ajouter une **règle...**

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.png)

12. Dans l’écran de l' **Assistant Ajouter une règle de revendication de transformation** , sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée** dans la liste déroulante, puis cliquez sur **suivant** .

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.png)

13. Dans l’écran de l' **Assistant Ajouter une règle de revendication de transformation** , entrez **ForCustomIDToken** dans nom de la **règle de revendication** et règle de revendication suivante dans **règle personnalisée**. Cliquez sur **Terminer**

    ```  
    x:[]
    => issue(claim=x);  
    ```

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap10.png)

```

>[!NOTE]
>You can also use PowerShell to assign the allatclaims and openid scopes
>``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "[Client ID from #3 above]" -ServerRoleIdentifier "[Identifier from #5 above]" -ScopeNames "allatclaims","openid"
```

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-id_token"></a>Télécharger et modifier l’exemple d’application pour émettre des revendications personnalisées dans id_token

Cette section explique comment télécharger l’exemple d’application Web et le modifier dans Visual Studio.   Nous allons utiliser l’exemple Azure AD qui se trouve [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Pour télécharger l’exemple de projet, utilisez git bash et tapez la commande suivante :  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>Pour modifier l’application

1.  Ouvrez l’exemple à l’aide de Visual Studio.  

2.  Régénérez l’application afin que toutes les packages NuGet manquantes soient restaurées.  

3.  Ouvrez le fichier Web. config.  Modifiez les valeurs suivantes pour qu’elles se présentent comme suit :  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 above under section Create and configure an Application Group in AD FS 2016 or later]" />  
    <add key="ida:ResourceID" value="[Replace this with the Web API Identifier from #5 above]"  />
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with the Redirect URI from #4 above]" />  
    ```  

    ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_2.PNG)  

4.  Ouvrez le fichier Startup.Auth.cs et apportez les modifications suivantes :  

    -   Ajustez la logique d’initialisation d’intergiciel OpenId Connect avec les modifications suivantes :  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
        private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

    -   Commentez ce qui suit :  

            ```  
            //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
            ```

          ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.PNG)

    -   Pour plus d’informations, modifiez les options de l’intergiciel OpenId Connect comme suit :  

        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                Resource = resourceId,
                MetadataAddress = metadataAddress,  
                PostLogoutRedirectUri = postLogoutRedirectUri,
                RedirectUri = postLogoutRedirectUri
        ```  

        ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_4.PNG)

5.  Ouvrez le fichier HomeController.cs et apportez les modifications suivantes :  

    -   Ajoutez ce qui suit :  

            ```  
            using System.Security.Claims;  
            ```

    -   Mettez à jour la méthode about () comme indiqué ci-dessous :  

        ```  
        [Authorize]
        public ActionResult About()
        {
            ClaimsPrincipal cp = ClaimsPrincipal.Current;
            string userName = cp.FindFirst(ClaimTypes.WindowsAccountName).Value;
            ViewBag.Message = String.Format("Hello {0}!", userName);
            return View();
        }
        ```  

        ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_5.PNG)

### <a name="test-the-custom-claims-in-id-token"></a>Tester les revendications personnalisées dans le jeton d’ID

Une fois les modifications apportées ci-dessus effectuées, appuyez sur F5. L’exemple de page s’affiche. Cliquez sur connexion.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

Vous serez redirigé vers la page de connexion AD FS. Continuez et connectez-vous.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

Une fois cette opération réussie, vous devriez voir que vous êtes maintenant connecté.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.PNG)

Cliquez sur à propos du lien. Vous verrez Hello [username] qui est récupéré à partir de la revendication de nom d’utilisateur dans le jeton d’ID

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.PNG)

## <a name="next-steps"></a>Étapes suivantes
[Développement des services AD FS](../../ad-fs/AD-FS-Development.md)  
