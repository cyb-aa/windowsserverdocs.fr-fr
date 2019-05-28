---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: Créer une application web à l’aide d’OpenID Connect avec AD FS 2016 et ultérieur
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbd42941f8952fc7f649636d2f3645f941240d49
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190418"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016-and-later"></a>Créer une application web à l’aide d’OpenID Connect avec AD FS 2016 et ultérieur

## <a name="pre-requisites"></a>Conditions préalables  
Voici une liste de conditions préalables qui sont nécessaires avant la fin de ce document. Ce document suppose que AD FS a été installé et une batterie de serveurs AD FS a été créé.  

-   Outils clients de GitHub  

-   AD FS dans Windows Server 2016 TP4 ou version ultérieure  

-   Visual Studio 2013 ou version ultérieure.  

## <a name="create-an-application-group-in-ad-fs-2016-and-later"></a>Créer un groupe d’applications dans AD FS 2016 et versions ultérieures
La section suivante décrit comment configurer l’application de groupe dans AD FS 2016 et versions ultérieure.  

#### <a name="create-application-group"></a>Créer le groupe d’applications  

1.  Dans Gestion AD FS, avec le bouton droit sur les groupes de l’Application et sélectionnez **ajouter l’Application groupe**.  

2.  Dans l’Assistant de groupe d’Application, pour le nom, entrez **ADFSSSO** et sous **applications Client-serveur** sélectionner le **navigateur Web l’accès à une application web** modèle.  Cliquez sur **Suivant**.

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  

3.  Copie le **identificateur Client** valeur.  Il servira ultérieurement comme valeur pour ida : ClientId dans le fichier web.config des applications.  

4.  Entrez les informations suivantes **URI de redirection :**  -  **https://localhost:44320/** .  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  

5.  Sur le **Résumé** , cliquez sur **suivant**.  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)

6.  Sur le **Terminer** , cliquez sur **fermer**.  

## <a name="download-and-modify-sample-application-to-authenticate-via-openid-connect-and-ad-fs"></a>Téléchargez et modifiez l’exemple d’application pour s’authentifier via OpenID Connect et AD FS  
Cette section explique comment télécharger l’exemple d’application Web et la modifier dans Visual Studio.   Nous allons utiliser l’exemple d’Azure AD est [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Pour télécharger l’exemple de projet, utilisez Git Bash et tapez la commande suivante :  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  

#### <a name="to-modify-the-app"></a>Pour modifier l’application  

1.  Ouvrez l’exemple à l’aide de Visual Studio.  

2.  Régénérer l’application afin que tous les packages NuGet manquants sont restaurés.  

3.  Ouvrez le fichier web.config.  Modifiez les valeurs suivantes afin du ressemblent à ce qui suit :  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 in above section]" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with Redirect URI from #4 in the above section]" />  
    ```  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  

4.  Ouvrez le fichier Startup.Auth.cs et apportez les modifications suivantes :  

    -   Commentez les éléments suivants :  

        ```  
        //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   Régler la logique d’initialisation intergiciel (middleware) OpenId Connect avec les modifications suivantes :  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  

    -   Vers le bas, modifier les options de middleware OpenId Connect comme suit :  

        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                MetadataAddress = metadataAddress,  
                PostLogoutRedirectUri = postLogoutRedirectUri,
                RedirectUri = postLogoutRedirectUri
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_11.PNG)  

        En modifiant la méthode ci-dessus, nous effectuons les éléments suivants :  

        -   Au lieu d’utiliser l’autorité pour la communication des données relatives à l’émetteur de confiance, nous spécifions l’emplacement du document de découverte directement via MetadataAddress  

        -   Azure AD n’applique pas la présence d’un redirect_uri dans la demande, contrairement à AD FS. Par conséquent, nous devons ajouter ici  

## <a name="verify-the-app-is-working"></a>Vérifiez que l’utilisation de l’application  
Une fois que les modifications ci-dessus ont été apportées, appuyez sur F5.  L’exemple de page s’affiche.  Cliquez sur connexion.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  

Vous serez redirigé vers la page de connexion AD FS.  Continuons et connectez-vous.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  

Une fois que l’opération réussit, vous devez voir que vous êtes désormais connecté.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  

## <a name="next-steps"></a>Étapes suivantes
[Développement des services AD FS](../../ad-fs/AD-FS-Development.md)  
