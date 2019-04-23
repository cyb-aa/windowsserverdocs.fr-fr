---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: Déploiement de serveurs proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dc49d8f4b656fdbb92083aa3c60bc4ce81091e9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890820"
---
# <a name="deploying-federation-server-proxies"></a>Déploiement de serveurs proxy de fédération

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Dans Active Directory Federation Services \(AD FS\) dans Windows Server 2012 R2, le rôle d’un serveur proxy de fédération est géré par un nouveau service de rôle accès à distance appelé Proxy d’Application Web. Pour activer AD FS pour l’accessibilité depuis l’extérieur du réseau d’entreprise, ce qui était l’objectif du déploiement d’un serveur proxy de fédération dans les versions héritées d’AD FS, telles que AD FS 2.0 et AD FS dans Windows Server 2012, vous pouvez déployer un ou plusieurs proxys d’application web pour A FS D dans Windows Server 2012 R2.  
  
Dans le contexte des services AD FS, le Proxy d’Application Web fonctionne comme un serveur proxy de fédération AD FS. De plus, il fournit une fonctionnalité de proxy inverse aux applications web de votre réseau d’entreprise, permettant aux utilisateurs de n’importe quel appareil d’y accéder depuis l’extérieur du réseau d’entreprise. Pour plus d’informations sur le proxy d’application web, consultez Vue d’ensemble du proxy d’application web.  
  
Pour planifier le déploiement de proxy d'Application Web, vous pouvez consulter les informations contenues dans les rubriques suivantes :  
  
-   [Planifier l’Infrastructure de Proxy d’Application Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Planifier le serveur de Proxy d’Application Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
Pour déployer le proxy d’application Web, vous pouvez suivre les procédures des rubriques suivantes :  
  
-   [Configurer l’Infrastructure de Proxy d’Application Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Installer et configurer le serveur de Proxy d’Application Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>Voir aussi 

[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

