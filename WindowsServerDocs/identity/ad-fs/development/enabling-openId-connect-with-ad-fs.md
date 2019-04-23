---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: Créer une application web à l’aide d’OpenID Connect avec AD FS 2016
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74a493e6568b71a05116140ec67586d36f439aa8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882660"
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016"></a>Créer une application web à l’aide d’OpenID Connect avec AD FS 2016

>S'applique à : Windows Server 2016

S’appuyant sur la prise en charge Oauth initiale dans AD FS dans Windows Server 2012 R2, AD FS 2016 introduit la prise en charge pour l’utilisation de la OpenId Connect sign-on.  
  
## <a name="pre-requisites"></a>Conditions préalables  
Voici une liste de conditions préalables qui sont nécessaires avant la fin de ce document. Ce document suppose que AD FS a été installé et une batterie de serveurs AD FS a été créé.  
  
-   Outils clients de GitHub  
  
-   AD FS dans Windows Server 2016 TP4 ou version ultérieure  
  
-   Visual Studio 2013 ou version ultérieure.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Créer un groupe d’applications dans AD FS 2016  
La section suivante décrit comment configurer le groupe d’applications dans AD FS 2016.  
  
#### <a name="create-application-group"></a>Créer le groupe d’applications  
  
1.  Dans Gestion AD FS, avec le bouton droit sur les groupes de l’Application et sélectionnez **ajouter l’Application groupe**.  
  
2.  Dans l’Assistant de groupe d’Application, pour le nom, entrez **ADFSSSO** et sous **applications autonomes** sélectionner le **application serveur ou un site Web** modèle.  Cliquez sur **Suivant**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  
  
3.  Copie le **identificateur Client** valeur.  Il servira ultérieurement comme valeur pour ida : ClientId dans le fichier web.config des applications.  
  
4.  Entrez les informations suivantes **URI de redirection :** - **https://localhost:44320/**.  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  
  
5.  Sur le **configurer les informations d’identification de l’Application** écran, cochez la case **générer un secret partagé** et copiez la clé secrète. Cliquez sur **Suivant**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)  
  
6.  Sur le **Résumé** , cliquez sur **suivant**.  
  
7.  Sur le **Terminer** , cliquez sur **fermer**.  
  
8.  Maintenant, sur le nouveau groupe d’applications avec le bouton droit et sélectionnez **propriétés**.  
  
9. Sur le **ADFSSSO propriétés** cliquez sur **ajouter application**.  
  
10. Sur le **ajouter une nouvelle application à l’exemple d’Application** sélectionnez **API Web** et cliquez sur **suivant**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_4.PNG)  
  
11. Sur le **configurer des API Web** écran, entrez les informations suivantes **identificateur** - **https://contoso.com/WebApp**.  Cliquez sur **Ajouter**. Cliquez sur **Suivant**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG) 
    
12. Sur le **choisir la stratégie de contrôle d’accès** s’affiche, sélectionnez **autoriser tout le monde** et cliquez sur **suivant**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. Sur le **configurer les autorisations Application** écran, vérifiez que **openid** est sélectionné, cliquez sur **suivant**.  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
14. Sur le **Résumé** , cliquez sur **suivant**.  
  
15. Sur le **Terminer** , cliquez sur **fermer**.  
  
16. Sur le **propriétés de l’Application exemple** cliquez sur **OK**.  
  
## <a name="download-and-modify-mvp-app-to-authenticate-via-openid-connect-and-ad-fs"></a>Téléchargez et modifiez l’application de MVP pour s’authentifier via OpenId vous connecter et AD FS  
Cette section explique comment télécharger l’exemple d’API Web et la modifier dans Visual Studio.   Nous allons utiliser l’exemple d’Azure AD est [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  
  
Pour télécharger l’exemple de projet, utilisez Git Bash et tapez la commande suivante :  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  
  
![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  
  
#### <a name="to-modify-the-app"></a>Pour modifier l’application  
  
1.  Ouvrez l’exemple à l’aide de Visual Studio.  
  
2.  Compilez l’application afin que tous les packages NuGet manquants sont restaurés.  
  
3.  Ouvrez le fichier web.config.  Modifiez les valeurs suivantes afin du ressemblent à ce qui suit :  
  
    ```  
    <add key="ida:ClientId" value="8219ab4a-df10-4fbd-b95a-8b53c1d8669e" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://adfs.contoso.com/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:ResourceID" value="https://contoso.com/WebApp"  
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="https://localhost:44320/" />  
    ```  
  
    ![AD FS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  
  
4.  Ouvrez le fichier Startup.Auth.cs et apportez les modifications suivantes :  
  
    -   Commentez les éléments suivants :  
  
        ```  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
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
  
    -   Plus bas, modifier les options de middleware OpenId Connect comme suit :  
  
        ```  
        app.UseOpenIdConnectAuthentication(  
            new OpenIdConnectAuthenticationOptions  
            {  
                ClientId = clientId,  
                //Authority = authority,  
                MetadataAddress = metadataAddress,  
                RedirectUri = postLogoutRedirectUri,  
                PostLogoutRedirectUri = postLogoutRedirectUri 
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
[Développement de AD FS](../../ad-fs/AD-FS-Development.md)  

