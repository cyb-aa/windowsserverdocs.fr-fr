---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: "Configurer un serveur de fédération avec Device Registration Service"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 511a039afd47cf7570fffdcaf17842e0eccc5683
ms.sourcegitcommit: 9278435cbfa8dbeb30d0557ed0d27832b154edd2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2018
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurer un serveur de fédération avec Device Registration Service

>S’applique à: Windows Server2012R2

Vous pouvez activer Device Registration Service \(DRS\) sur votre serveur de fédération après avoir effectué les procédures de [étape4: configurer un serveur de fédération](https://technet.microsoft.com/library/dn303424.aspx). Le Service d’inscription de périphérique fournit un mécanisme d’intégration pour transparente facteur d’authentification, persistantes seul sur connexion \(SSO\) et accès conditionnel aux consommateurs qui nécessitent un accès aux ressources d’entreprise. Pour plus d’informations sur DRS, voir [rejoindre un espace de travail à partir de n’importe quel appareil pour l’authentification unique et transparente deuxième facteur d’authentification entre les Applications d’entreprise](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Préparer votre forêt ActiveDirectory pour prendre en charge des périphériques  
  
> [!NOTE]  
> Il s’agit d’une opération one\-heure que vous devez exécuter pour préparer votre forêt ActiveDirectory pour prendre en charge des périphériques. Vous devez être connecté avec les autorisations d’administrateur d’entreprise et votre forêt ActiveDirectory doit avoir le schéma de Windows Server2012R2 pour effectuer cette procédure.  
>   
> En outre, DRS requiert que vous disposez d’au moins un serveur de catalogue global dans votre domaine racine de forêt. Le serveur de catalogue global est requis pour pouvoir exécuter Blob\-ADDeviceRegistration et lors de l’authentification ADFS. ADFS Initialise une représentation de la mémoire ou de l’objet de configuration du service DRS sur chaque demande d’authentification et si l’objet de configuration de DRS est introuvable sur un contrôleur de domaine dans le domaine actuel, la demande est effectuée sur le catalogue global sur lequel les objets DRS ont été configurés au cours de Blob\-ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Pour préparer la forêt ActiveDirectory  
  
1.  Sur votre serveur de fédération, ouvrez une fenêtre de commande Windows PowerShell et tapez:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Lorsque vous y êtes invité pour ServiceAccountName, entrez le nom du compte de service que vous avez sélectionné comme compte de service pour ADFS.  Si elle est un compte de service administré de groupe, entrez le compte dans le **domain\\accountname$** format. Pour un compte de domaine, utilisez le format **domain\\accountname**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Activer Device Registration Service sur un nœud de la batterie de serveurs de fédération  
  
> [!NOTE]  
> Vous devez être connecté avec des autorisations d’administrateur de domaine pour effectuer cette procédure.  
  
#### <a name="to-enable-device-registration-service"></a>Pour activer le Service d’inscription de périphérique  
  
1.  Sur votre serveur de fédération, ouvrez une fenêtre de commande Windows PowerShell et tapez:  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Répétez cette procédure sur chaque nœud de la batterie de serveurs de fédération dans votre batterie ADFS.  
  
## <a name="enable-seamless-second-factor-authentication"></a>Activer transparente authentification second facteur  
Transparente authentification second facteur est une amélioration dans ADFS qui fournit un niveau supplémentaire de protection d’accès aux ressources d’entreprise et applications à partir d’appareils externes, essayez d’y accéder. Lorsqu’un appareil personnel est joint au lieu de travail, il devient un appareil «connu» et les administrateurs peuvent utiliser ces informations pour l’accès conditionnel et permettent l’accès aux ressources.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Pour activer transparente à deux facteurs l’authentification, persistantes seul sur connexion \(SSO\) et accès conditionnel pour les appareils joints au lieu de travail  
  
1.  Dans la console de gestion ADFS, accédez aux stratégies d’authentification. Sélectionnez Modifier l’authentification principale globale. Sélectionnez la case à cocher en regard d’activer l’authentification des appareils, puis cliquez sur OK.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Mise à jour la configuration du Proxy d’Application Web  
  
> [!IMPORTANT]  
> Vous n’avez pas besoin de publier le Service d’inscription de périphérique pour le Proxy d’Application Web.  Le Service d’inscription de périphérique sera disponible via le Proxy d’Application Web une fois qu’il est activé sur un serveur de fédération.  Vous devrez peut-être effectuer cette procédure pour mettre à jour la configuration du Proxy d’Application Web s’il a été déployé avant d’activer le Service d’inscription de périphérique.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Pour mettre à jour la Configuration du Proxy d’Application Web  
  
1.  Sur votre serveur Proxy d’Application Web, ouvrez une fenêtre de commande Windows PowerShell et tapez  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Lorsque vous y êtes invité pour des informations d’identification, entrez les informations d’identification d’un compte qui dispose de droits d’administration pour vos serveurs de fédération.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server2012R2 ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

