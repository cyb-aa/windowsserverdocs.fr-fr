---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: Configurer un serveur de fédération avec Device Registration Service
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 511a039afd47cf7570fffdcaf17842e0eccc5683
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843060"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurer un serveur de fédération avec Device Registration Service

>S'applique à : Windows Server 2012 R2

Vous pouvez activer le Service Device Registration \(DRS\) sur votre serveur de fédération après avoir terminé les procédures de [étape 4 : Configurer un serveur de fédération](https://technet.microsoft.com/library/dn303424.aspx). Le Service Device Registration fournit un mécanisme de l’intégration pour transparente l’authentification multifacteur, l’authentification unique persistante\-sur \(SSO\)et l’accès conditionnel à des consommateurs qui requièrent l’accès à l’entreprise ressources. Pour plus d’informations sur DRS, consultez [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Préparer votre forêt Active Directory pour prendre en charge des appareils  
  
> [!NOTE]  
> Il s’agit d’un\-temps l’opération que vous devez exécuter pour préparer votre forêt Active Directory pour prendre en charge des appareils. Vous devez être connecté avec des autorisations d’administrateur d’entreprise et votre forêt Active Directory doit avoir le schéma de Windows Server 2012 R2 pour effectuer cette procédure.  
>   
> En outre, DRS requiert que vous disposez d’au moins un serveur de catalogue global dans votre domaine racine de forêt. Le serveur de catalogue global est obligatoire pour exécuter Initialize\-ADDeviceRegistration et lors de l’authentification AD FS. AD FS Initialise un dans\-sur chaque demande d’authentification de l’objet de représentation de la mémoire de la configuration DRS et si l’objet de configuration de DRS ne peut pas être trouvée sur un contrôleur de domaine dans le domaine actuel, la demande est tentée contre le GC sur lequel se trouvent les objets de DRS mis en service pendant l’initialisation\-ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Pour préparer la forêt Active Directory  
  
1.  Sur votre serveur de fédération, ouvrez une fenêtre de commande Windows PowerShell et tapez :  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Lorsque vous y êtes invité pour ServiceAccountName, entrez le nom du compte de service que vous avez sélectionné en tant que le compte de service pour AD FS.  Dans le cas d’un compte gMSA, entrez le compte dans le **domaine\\accountname$** format. Pour un compte de domaine, utilisez le format **domaine\\accountname**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Activer le Service Device Registration sur un nœud de batterie de serveurs de fédération  
  
> [!NOTE]  
> Vous devez être connecté avec des autorisations d’administrateur de domaine pour effectuer cette procédure.  
  
#### <a name="to-enable-device-registration-service"></a>Pour activer le Service Device Registration  
  
1.  Sur votre serveur de fédération, ouvrez une fenêtre de commande Windows PowerShell et tapez :  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Répétez cette étape sur chaque nœud de batterie de serveurs de fédération dans votre batterie de serveurs AD FS...  
  
## <a name="enable-seamless-second-factor-authentication"></a>Activation transparente authentification de second facteur  
Transparente authentification de second facteur est une amélioration dans AD FS qui fournit un niveau de protection d’accès aux ressources d’entreprise et aux applications à partir d’appareils externes qui tentent d’y accéder. Quand un appareil personnel est un espace de travail, il devient un dispositif « connu » et les administrateurs peuvent utiliser ces informations pour piloter l’accès conditionnel et réguler l’accès aux ressources.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Pour activer la deuxième transparente-factor authentication, persistant l’authentification unique\-sur \(SSO\) et l’accès conditionnel pour les appareils avec Workplace JOIN  
  
1.  Dans la console de gestion AD FS, accédez à stratégies d’authentification. Sélectionnez Modifier l’authentification principale globale. Sélectionnez la case à cocher en regard d’activer l’authentification des appareils, puis cliquez sur OK.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Mise à jour la configuration de Proxy d’Application Web  
  
> [!IMPORTANT]  
> Il est inutile de publier le Service Device Registration sur le Proxy d’Application Web.  Le Service d’inscription de périphérique sera disponible via le Proxy d’Application Web une fois qu’il est activé sur un serveur de fédération.  Vous devrez peut-être effectuer cette procédure pour mettre à jour la configuration de Proxy d’Application Web si elle a été déployée avant d’activer le Service d’inscription de périphérique.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Pour mettre à jour la Configuration de Proxy d’Application Web  
  
1.  Sur votre serveur Proxy d’Application Web, ouvrez une fenêtre de commande Windows PowerShell et tapez  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Lorsque vous y êtes invité pour les informations d’identification, entrez les informations d’identification d’un compte disposant des droits d’administration sur les serveurs de fédération.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

