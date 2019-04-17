---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: "Déploiement de serveurs proxy de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dc49d8f4b656fdbb92083aa3c60bc4ce81091e9b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-server-proxies"></a>Déploiement de serveurs proxy de fédération

>S’applique à: Windows Server2016, Windows Server2012R2

Dans ActiveDirectory Federation Services \(ADFS\) dans Windows Server2012R2, le rôle d’un serveur proxy de fédération est géré par un nouveau service de rôle accès à distance appelé Proxy d’Application Web. Pour activer ADFS pour l’accessibilité depuis l’extérieur du réseau d’entreprise, qui était l’objectif du déploiement d’un serveur proxy de fédération dans les versions héritées de ADFS, telles que les services ADFS 2.0 et ADFS dans Windows Server2012, vous pouvez déployer un ou plusieurs proxys d’application web pour ADFS dans Windows Server2012R2.  
  
Dans le contexte d’ADFS, le Proxy d’Application Web fonctionne comme un serveur proxy de fédération ADFS. En outre, le Proxy d’Application Web fournit la fonctionnalité de proxy inverse pour les applications web à l’intérieur de votre réseau d’entreprise permettre aux utilisateurs sur n’importe quel appareil d’y accéder depuis l’extérieur du réseau d’entreprise. Pour plus d’informations sur le service de rôle Proxy d’Application Web, voir vue d’ensemble du Proxy d’Application Web.  
  
Pour planifier le déploiement de proxy d’Application Web, vous pouvez passer en revue les informations contenues dans les rubriques suivantes:  
  
-   [Planifier l’Infrastructure du Proxy d’Application Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Planifier le serveur de Proxy d’Application Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
Pour déployer le proxy d’Application Web, vous pouvez suivre les procédures décrites dans les rubriques suivantes:  
  
-   [Configurer l’Infrastructure du Proxy d’Application Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Installer et configurer le serveur de Proxy d’Application Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>Voir aussi 

[Déploiement d’ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server2012R2 ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

