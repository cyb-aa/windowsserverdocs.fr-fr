---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: Créer une application Web à l’aide de OpenID Connect avec AD FS 2016 et versions ultérieures
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 49d952a49cf474708f57a0ae2a7760d2470af607
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857492"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016-and-later"></a>Créer une application Web à l’aide de OpenID Connect avec AD FS 2016 et versions ultérieures

## <a name="pre-requisites"></a>Prérequis  
La liste suivante répertorie les conditions préalables requises avant de terminer ce document. Ce document suppose que AD FS a été installé et qu’une batterie de AD FS a été créée.  

-   Outils clients GitHub  

-   AD FS dans Windows Server 2016 TP4 ou version ultérieure  

-   Visual Studio 2013 ou version ultérieure.  

## <a name="create-an-application-group-in-ad-fs-2016-and-later"></a>Créer un groupe d’applications dans AD FS 2016 et versions ultérieures
La section suivante décrit comment configurer le groupe d’applications dans AD FS 2016 et versions ultérieures.  

#### <a name="create-application-group"></a>Créer un groupe d’applications  

1.  Dans AD FS gestion, cliquez avec le bouton droit sur groupes d’applications, puis sélectionnez **Ajouter un groupe d’applications**.  

2.  Dans l’Assistant groupe d’applications, pour le nom, entrez **ADFSSSO** , puis sous **applications client-serveur** , sélectionnez le **navigateur Web accédant à un modèle d’application Web** .  Cliquez sur **Suivant**.

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  

3.  Copiez la valeur de l' **identificateur du client** .  Il sera utilisé ultérieurement comme valeur pour Ida : ClientId dans le fichier Web. config des applications.  

4.  Entrez les informations suivantes pour l' **URI de redirection :**  -  **https://localhost:44320/** .  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  

5.  Dans l’écran **Résumé** , cliquez sur **suivant**.  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)

6.  Dans l’écran **terminé** , cliquez sur **Fermer**.  

## <a name="download-and-modify-sample-application-to-authenticate-via-openid-connect-and-ad-fs"></a>Télécharger et modifier l’exemple d’application pour l’authentification via OpenID Connect et AD FS  
Cette section explique comment télécharger l’exemple d’application Web et le modifier dans Visual Studio.   Nous allons utiliser l’exemple Azure AD qui se trouve [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  

Pour télécharger l’exemple de projet, utilisez git bash et tapez la commande suivante :  

```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  

#### <a name="to-modify-the-app"></a>Pour modifier l’application  

1.  Ouvrez l’exemple à l’aide de Visual Studio.  

2.  Régénérez l’application afin que toutes les packages NuGet manquantes soient restaurées.  

3.  Ouvrez le fichier Web. config.  Modifiez les valeurs suivantes pour qu’elles se présentent comme suit :  

    ```  
    <add key="ida:ClientId" value="[Replace this Client Id from #3 in above section]" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://[Your ADFS hostname]/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="[Replace this with Redirect URI from #4 in the above section]" />  
    ```  

    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  

4.  Ouvrez le fichier Startup.Auth.cs et apportez les modifications suivantes :  

    -   Commentez ce qui suit :  

        ```  
        //string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  

    -   Ajustez la logique d’initialisation d’intergiciel OpenId Connect avec les modifications suivantes :  

        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  

        ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  

    -   Pour plus d’informations, modifiez les options de l’intergiciel OpenId Connect comme suit :  

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

        En modifiant les éléments ci-dessus, nous procédons comme suit :  

        -   Au lieu d’utiliser l’autorité pour communiquer les données relatives à l’émetteur approuvé, nous spécifions l’emplacement du document de découverte directement via MetadataAddress  

        -   Azure AD n’impose pas la présence d’un redirect_uri dans la requête, contrairement à ADFS. Nous devons donc l’ajouter ici  

## <a name="verify-the-app-is-working"></a>Vérifier que l’application fonctionne  
Une fois les modifications apportées ci-dessus effectuées, appuyez sur F5.  L’exemple de page s’affiche.  Cliquez sur connexion.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  

Vous serez redirigé vers la page de connexion AD FS.  Continuez et connectez-vous.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  

Une fois cette opération réussie, vous devriez voir que vous êtes maintenant connecté.  

![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  

## <a name="next-steps"></a>Étapes suivantes
[Développement des services AD FS](../../ad-fs/AD-FS-Development.md)  
