---
title: Personnaliser des revendications pour être émis dans id_token lors de l’utilisation de OpenID Connect ou OAuth avec AD FS 2016 ou version ultérieure
description: Une vue d’ensemble technique des concepts de jeton d’id personnalisé dans AD FS 2016 ou version ultérieure
author: anandyadavmsft
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.reviewer: anandy
ms.technology: identity-adfs
ms.openlocfilehash: c4f9a2880aa91b7a600cdb40238bead7d565e6bc
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65977026"
---
# <a name="customize-claims-to-be-emitted-in-idtoken-when-using-openid-connect-or-oauth-with-ad-fs-2016-or-later"></a>Personnaliser des revendications pour être émis dans id_token lors de l’utilisation de OpenID Connect ou OAuth avec AD FS 2016 ou version ultérieure

## <a name="overview"></a>Vue d'ensemble
L’article [ici](native-client-with-ad-fs.md) montre comment créer une application qui utilise ADFS pour OpenID Connect d’authentification. Toutefois, par défaut il existe uniquement un ensemble fixe de revendications disponibles dans le paramètre id_token. AD FS 2016 et versions ultérieures ont la possibilité de personnaliser id_token dans les scénarios OpenID Connect.

## <a name="when-are-custom-id-token-used"></a>Lorsque sont ID personnalisé jeton utilisé ?
Dans certains scénarios, il est possible que l’application cliente n’a pas d’une ressource qu’il tente d’accéder. Par conséquent, il n’a pas vraiment besoin d’un jeton d’accès. Dans ce cas, l’application cliente doit essentiellement uniquement un ID jeton, mais certaines revendications supplémentaires pour vous aider dans la fonctionnalité.

## <a name="what-are-the-restrictions-on-getting-custom-claims-in-id-token"></a>Quelles sont les restrictions sur l’obtention des revendications personnalisées dans l’ID de jeton ?

### <a name="scenario-1"></a>Scénario 1

![restreindre](media/Custom-Id-Tokens-in-AD-FS/res1.png)

1.  response_mode est défini comme form_post
2.  Uniquement les clients publics peuvent obtenir des revendications personnalisées dans l’ID de jeton
3.  Identificateur de partie de confiance tiers (identificateur de l’API Web) doit être identique en tant qu’identificateur de client

### <a name="scenario-2"></a>Scénario 2

![restreindre](media/Custom-Id-Tokens-in-AD-FS/restrict2.png)

Avec [KB4019472](https://support.microsoft.com/help/4019472/windows-10-update-kb4019472) installé sur vos serveurs AD FS
1.  response_mode est défini comme form_post
2.  Publics et confidentielles les clients peuvent obtenir des revendications personnalisées dans l’ID de jeton
3.  Affecter allatclaims d’étendue pour le client : paire de partie de confiance.
Vous pouvez affecter l’étendue à l’aide de l’applet de commande Grant-ADFSApplicationPermission, comme indiqué dans l’exemple ci-dessous :

``` powershell
Grant-AdfsApplicationPermission -ClientRoleIdentifier "https://my/privateclient" -ServerRoleIdentifier "https://rp/fedpassive" -ScopeNames "allatclaims","openid"
```

## <a name="creating-and-configuring-an-oauth-application-to-handle-custom-claims-in-id-token"></a>Création et configuration d’une application OAuth pour gérer les revendications personnalisées dans le jeton d’ID
Suivez les étapes ci-dessous pour créer et configurer l’application dans AD FS pour la réception du jeton d’ID avec des revendications personnalisées.

### <a name="create-and-configure-an-application-group-in-ad-fs-2016-or-later"></a>Créer et configurer un groupe d’applications dans AD FS 2016 ou version ultérieure

1. Dans Gestion AD FS, avec le bouton droit sur les groupes de l’Application et sélectionnez **ajouter l’Application groupe**.

2. Dans l’Assistant de groupe d’Application, pour le nom, entrez **ADFSSSO** et sélectionnez des applications sous Client-serveur le **application Native l’accès à une application web** modèle. Cliquez sur **Suivant**.

  ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap1.png)

3. Copie le **identificateur Client** valeur.  Il servira ultérieurement comme valeur pour ida : ClientId dans le fichier web.config des applications.

4. Entrez les informations suivantes **URI de redirection :**  -  **https://localhost:44320/** .  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.

  ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap2.png)

5. Sur le **configurer des API Web** écran, entrez les informations suivantes **identificateur** -  **https://contoso.com/WebApp** .  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.  Cette valeur sera utilisée ultérieurement pour **ida : ResourceID** dans le fichier web.config des applications.

  ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap3.png)

6. Sur le **choisir la stratégie de contrôle d’accès** s’affiche, sélectionnez **autoriser tout le monde** et cliquez sur **suivant**.

  ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap4.png)

7. Sur le **configurer les autorisations Application** écran, vérifiez que **openid** et **allatclaims** sont sélectionnées et cliquez sur **suivant**.

  ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap5.png)

8. Sur le **Résumé** , cliquez sur **suivant**.  

  ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap6.png)

9. Sur le **Terminer** , cliquez sur **fermer**.

10. Dans Gestion AD FS, cliquez sur groupes de l’Application pour obtenir la liste de tous les groupes de l’application. Avec le bouton droit sur **ADFSSSO** et sélectionnez **propriétés**. Sélectionnez **ADFSSSO - API Web** et cliquez sur **modifier...**

    ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap7.png)

11. Sur **ADFSSSO - propriétés d’API Web** s’affiche, sélectionnez **règles de transformation d’émission** onglet et cliquez sur **ajouter une règle...**

  ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap8.png)

12. Sur **ajouter un Assistant de règle de revendication de transformation** s’affiche, sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée** à partir de la liste déroulante et cliquez sur **suivant**

  ![Client](media/Custom-Id-Tokens-in-AD-FS/clientsnap9.png)

13. Sur **ajouter transformer Assistant règle de revendication** écran, entrez **ForCustomIDToken** dans **nom de règle de revendication** et de la règle de revendication suivant **règle personnalisée**. Cliquez sur **terminer**

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

### <a name="download-and-modify-the-sample-application-to-emit-custom-claims-in-idtoken"></a>Téléchargez et modifiez l’exemple d’application pour émettre des revendications personnalisées dans id_token

Cette section explique comment télécharger l’exemple d’application Web et la modifier dans Visual Studio.   Nous allons utiliser l’exemple d’Azure AD est [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Pour télécharger l’exemple de projet, utilisez Git Bash et tapez la commande suivante :  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_1.PNG)

#### <a name="to-modify-the-app"></a>Pour modifier l’application

1.  Ouvrez l’exemple à l’aide de Visual Studio.  

2.  Régénérer l’application afin que tous les packages NuGet manquants sont restaurés.  

3.  Ouvrez le fichier web.config.  Modifiez les valeurs suivantes afin du ressemblent à ce qui suit :  

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

    -   Régler la logique d’initialisation intergiciel (middleware) OpenId Connect avec les modifications suivantes :  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];
        private static string resourceId = ConfigurationManager.AppSettings["ida:ResourceID"];
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

    -   Commentez les éléments suivants :  

            ```  
            //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
            ```

          ![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_3.PNG)

    -   Vers le bas, modifier les options de middleware OpenId Connect comme suit :  

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

    -   Mettre à jour la méthode About() comme indiqué ci-dessous :  

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

Une fois que les modifications ci-dessus ont été apportées, appuyez sur F5. L’exemple de page s’affiche. Cliquez sur connexion.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_6.PNG)

Vous serez redirigé vers la page de connexion AD FS. Continuons et connectez-vous.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_7.PNG)

Une fois que l’opération réussit, vous devez voir que vous êtes désormais connecté.

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_8.PNG)

Cliquez sur le lien. Vous verrez Hello [Username] qui est récupéré à partir de la revendication de nom d’utilisateur dans le jeton d’ID

![AD FS OpenID](media/Custom-Id-Tokens-in-AD-FS/AD_FS_OpenID_9.PNG)

## <a name="next-steps"></a>Étapes suivantes
[Développement des services AD FS](../../ad-fs/AD-FS-Development.md)  
