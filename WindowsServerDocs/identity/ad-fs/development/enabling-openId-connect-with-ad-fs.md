---
ms.assetid: d282bb4e-38a0-4c7c-83d8-f6ea89278057
title: "Créer une application web à l’aide de OpenID se connecter avec ADFS2016"
description: 
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f8040b19576ac9de4ced43e6313cad69276a3d27
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2018
---
# <a name="build-a-web-application-using-openid-connect-with-ad-fs-2016"></a>Créer une application web à l’aide de OpenID se connecter avec ADFS2016

>S’applique à: Windows Server2016

S’appuyant sur la prise en charge Oauth initiale dans ADFS dans Windows Server2012R2, ADFS2016 prend en charge à l’aide de la connexion OpenId ouverture de session.  
  
## <a name="pre-requisites"></a>Conditions préalables  
Voici une liste des conditions préalables requises avant la fin de ce document. Ce document suppose que ADFS a été installé et une batterie ADFS a été créée.  
  
-   Un abonnement Azure AD (une version d’évaluation gratuite est parfaite)  
  
-   Outils du client GitHub  
  
-   ADFS dans Windows Server2016 TP4 ou version ultérieure  
  
-   Visual Studio2013 ou version ultérieure.  
  
## <a name="create-an-application-group-in-ad-fs-2016"></a>Créer un groupe d’applications dans ADFS2016  
La section suivante décrit comment configurer le groupe d’applications dans ADFS2016.  
  
#### <a name="create-application-group"></a>Créer le groupe d’applications  
  
1.  Dans Gestion ADFS, avec le bouton droit sur les groupes d’applications et sélectionnez **ajouter un groupe d’Application**.  
  
2.  Dans l’Assistant groupe d’Application, pour le nom, entrez **ADFSSSO** sous **applications autonomes**sélectionner le **application serveur ou le site Web** modèle.  Cliquez sur **suivant**.  
  
    ![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_1.PNG)  
  
3.  Copie le **identificateur de Client** valeur.  Il sera être utilisé ultérieurement comme valeur pour ida: ClientId dans le fichier web.config d’application.  
  
4.  Entrez les informations suivantes pour **URI de redirection:** - **https://localhost:44320/**.  Cliquez sur **ajouter**. Cliquez sur **suivant**.  
  
    ![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_2.PNG)  
  
5.  Sur le **configurer les informations d’identification de l’Application** écran, placez la case à cocher **générer un secret partagé** et copiez la clé secrète. Cliquez sur **suivant**  
  
    ![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_3.PNG)  
  
6.  Sur le **Résumé**, cliquez sur **suivant**.  
  
7.  Sur le **Complete**, cliquez sur **fermer**.  
  
8.  Maintenant, sur le nouveau groupe d’Application avec le bouton droit et sélectionnez **propriétés**.  
  
9. Sur le **ADFSSSO propriétés** cliquez sur **ajouter application**.  
  
10. Sur le **ajouter une nouvelle application à l’exemple d’Application** sélectionnez **API Web** et cliquez sur **suivant**.  
  
    ![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_4.PNG)  
  
11. Sur le **configurer des API Web** de l’écran, entrez les informations suivantes pour **identificateur** - **https://contoso.com/WebApp**.  Cliquez sur **ajouter**. Cliquez sur **suivant**.  
  
    ![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
12. Sur le **stratégie de contrôle d’accès choisir** écran, puis sélectionnez **autoriser tout le monde** et cliquez sur **suivant**.  
  
    ![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_Confidential_7.PNG)  
  
13. Sur le **configurer des autorisations Application** écran, assurez-vous que **openid** est sélectionné, cliquez sur **suivant**.  
  
    ![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_7.PNG)  
  
14. Sur le **Résumé**, cliquez sur **suivant**.  
  
15. Sur le **Complete**, cliquez sur **fermer**.  
  
16. Sur le **propriétés de l’Application exemple** cliquez sur **OK**.  
  
## <a name="download-and-modify-mvp-app-to-authenticate-via-openid-connect-and-ad-fs"></a>Télécharger et modifier l’application MVP pour l’authentification via OpenId se connecter et ADFS  
Cette section explique comment télécharger l’exemple API Web et la modifier dans Visual Studio.   Nous allons utiliser l’exemple d’Azure AD qui est [ici](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect).  
  
Pour télécharger l’exemple de projet, utiliser Git Bash et tapez la commande suivante:  
  
```  
git clone https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect  
```  
  
![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_8.PNG)  
  
#### <a name="to-modify-the-app"></a>Pour modifier l’application  
  
1.  Ouvrez l’exemple à l’aide de Visual Studio.  
  
2.  Compiler l’application afin que tous les NuGets manquants sont restaurés.  
  
3.  Ouvrez le fichier web.config.  Modifiez les valeurs suivantes pour l’apparence comme suit:  
  
    ```  
    <add key="ida:ClientId" value="8219ab4a-df10-4fbd-b95a-8b53c1d8669e" />  
    <add key="ida:ADFSDiscoveryDoc" value="https://adfs.contoso.com/adfs/.well-known/openid-configuration" />  
    <!--<add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" />      
    <add key="ida:ResourceID" value="https://contoso.com/WebApp"  
    <add key="ida:AADInstance" value="https://login.microsoftonline.com/{0}" />-->  
    <add key="ida:PostLogoutRedirectUri" value="https://localhost:44320/" />  
    ```  
  
    ![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_9.PNG)  
  
4.  Ouvrez le fichier Startup.Auth.cs et apportez les modifications suivantes:  
  
    -   Commentez les éléments suivants:  
  
        ```  
        //public static readonly string Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);  
        ```  
  
    -   Modifier la logique de l’initialisation des intergiciels (middleware) OpenId se connecter avec les modifications suivantes:  
  
        ```  
        private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];  
        //private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];  
        //private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];  
        private static string metadataAddress = ConfigurationManager.AppSettings["ida:ADFSDiscoveryDoc"];  
        private static string postLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];  
        ```  
  
        ![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_10.PNG)  
  
    -   Plus loin vers le bas, modifier les options de middleware OpenId connecter comme dans les éléments suivants:  
  
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
  
        ![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_11.PNG)  
  
        En modifiant les informations ci-dessus, nous effectuons les éléments suivants:  
  
        -   Au lieu d’utiliser l’autorité de communication de données sur l’émetteur approuvé, nous spécifions l’emplacement du document de découverte directement via MetadataAddress  
  
        -   Azure AD n’applique pas la présence d’un redirect_uri dans la demande, mais ADFS. Par conséquent, nous devons l’ajouter ici  
  
## <a name="verify-the-app-is-working"></a>Vérifier le que fonctionne de l’application  
Une fois que les modifications ci-dessus ont été apportées, appuyez sur F5.  L’exemple de page s’affiche.  Cliquez sur connexion.  
  
![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_12.PNG)  
  
Vous serez redirigé vers la page de connexion ADFS.  Lancez-vous et connectez-vous.  
  
![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_13.PNG)  
  
Une fois que l’opération réussit, vous devez voir que vous êtes maintenant connecté.  
  
![ADFS OpenID](media/Enabling-OpenId-Connect-with-AD-FS-2016/AD_FS_OpenID_14.PNG)  
  
## <a name="next-steps"></a>Étapes suivantes
[Développement d’ADFS](../../ad-fs/AD-FS-Development.md)  

