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
ms.openlocfilehash: 12676d40d52046ae4ff2fe83c199ad21db4cf8ab
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868082"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurer un serveur de fédération avec Device Registration Service

Vous pouvez activer Device Registration service \(Drs\) sur votre serveur de Fédération une fois que vous avez [terminé les procédures de l’étape 4 : Configurez un](https://technet.microsoft.com/library/dn303424.aspx)serveur de Fédération. Device Registration service fournit un mécanisme d’intégration pour l’authentification de second facteur transparente, l’authentification\-unique \(\)permanente et l’accès conditionnel aux consommateurs qui requièrent l’accès à la société. situées. Pour plus d’informations sur DRS, consultez [joindre à l’espace de travail à partir de n’importe quel appareil pour l’authentification unique et l’authentification de second facteur transparente pour les applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md) de l’entreprise  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Préparer votre forêt Active Directory pour la prise en charge des appareils  
  
> [!NOTE]  
> Il s’agit d'\-une opération ponctuelle que vous devez exécuter pour préparer votre forêt Active Directory à prendre en charge des appareils. Pour effectuer cette procédure, vous devez avoir ouvert une session avec les autorisations d’administrateur d’entreprise et votre forêt Active Directory doit avoir le schéma Windows Server 2012 R2.  
>   
> De plus, DRS nécessite que vous disposiez d’au moins un serveur de catalogue global dans votre domaine racine de forêt. Le serveur de catalogue global est requis pour exécuter Initialize\-ADDeviceRegistration et Pendant l’authentification AD FS. AD FS Initialise une représentation en\-mémoire de l’objet de configuration Drs à chaque demande d’authentification et si l’objet de configuration Drs est introuvable sur un contrôleur de domaine dans le domaine actuel, la demande est tentée par rapport au GC sur lequel les objets Drs étaient approvisionné lors de l’initialisation\-de ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Pour préparer la forêt Active Directory  
  
1.  Sur votre serveur de Fédération, ouvrez une fenêtre de commande Windows PowerShell et tapez :  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Lorsque vous êtes invité à entrer des ServiceAccountName, entrez le nom du compte de service que vous avez sélectionné en tant que compte de service pour AD FS.  S’il s’agit d’un compte gMSA, entrez le compte dans le format de **\\domaine AccountName $** . Pour un compte de domaine, utilisez le **format\\domaine AccountName**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Activer Device Registration service sur un nœud de batterie de serveurs de Fédération  
  
> [!NOTE]  
> Pour effectuer cette procédure, vous devez avoir ouvert une session avec les autorisations d’administrateur de domaine.  
  
#### <a name="to-enable-device-registration-service"></a>Pour activer Device Registration service  
  
1.  Sur votre serveur de Fédération, ouvrez une fenêtre de commande Windows PowerShell et tapez :  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Répétez cette étape sur chaque nœud de la batterie de serveurs de Fédération dans votre batterie de AD FS.  
  
## <a name="enable-seamless-second-factor-authentication"></a>Activer l’authentification de second facteur transparente  
L’authentification de second facteur transparente est une amélioration de AD FS qui fournit un niveau supplémentaire de protection d’accès aux ressources et applications d’entreprise à partir d’appareils externes qui essaient d’y accéder. Lorsqu’un appareil personnel est joint à un espace de travail, il devient un appareil « connu » et les administrateurs peuvent utiliser ces informations pour accéder aux ressources en matière d’accès conditionnel et de passerelle.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Pour activer l’authentification de second facteur transparente, l'\-authentification \(unique\) permanente et l’accès conditionnel pour les appareils joints à l’espace de travail  
  
1.  Dans la console de gestion AD FS, accédez à stratégies d’authentification. Sélectionnez modifier l’authentification principale globale. Activez la case à cocher en regard de activer l’authentification des appareils, puis cliquez sur OK.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Mettre à jour la configuration du proxy d’application Web  
  
> [!IMPORTANT]  
> Vous n’avez pas besoin de publier le service Device Registration sur le proxy d’application Web.  Le service d’inscription d’appareils sera disponible via le proxy d’application Web une fois qu’il est activé sur un serveur de Fédération.  Vous devrez peut-être effectuer cette procédure pour mettre à jour la configuration du proxy d’application Web si elle a été déployée avant d’activer le service d’inscription d’appareils.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Pour mettre à jour la configuration du proxy d’application Web  
  
1.  Sur votre serveur proxy d’application Web, ouvrez une fenêtre de commande Windows PowerShell et tapez  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Lorsque vous êtes invité à entrer vos informations d’identification, entrez les informations d’identification d’un compte disposant de droits d’administration sur vos serveurs de Fédération.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

