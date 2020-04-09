---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: Déploiement de serveurs proxy de fédération
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 04837c8b38f1f6cdf048f7d8744d9cd0f61ebb0d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855452"
---
# <a name="deploying-federation-server-proxies"></a>Déploiement de serveurs proxy de fédération

Dans Services ADFS \(AD FS\) dans Windows Server 2012 R2, le rôle de serveur proxy de Fédération est géré par un nouveau service de rôle accès à distance appelé proxy d’application Web. Pour activer votre AD FS pour l’accessibilité depuis l’extérieur du réseau d’entreprise, ce qui était l’objectif du déploiement d’un serveur proxy de Fédération dans les versions héritées de AD FS, comme AD FS 2,0 et AD FS dans Windows Server 2012, vous pouvez déployer un ou plusieurs proxys d’application Web pour AD FS dans Windows Server 2012 R2.  
  
Dans le contexte de AD FS, le proxy d’application Web fonctionne comme un proxy de serveur de fédération AD FS. De plus, il fournit une fonctionnalité de proxy inverse aux applications web de votre réseau d’entreprise, permettant aux utilisateurs de n’importe quel appareil d’y accéder depuis l’extérieur du réseau d’entreprise. Pour plus d’informations sur le proxy d’application web, consultez Vue d’ensemble du proxy d’application web.  
  
Pour planifier le déploiement de proxy d'Application Web, vous pouvez consulter les informations contenues dans les rubriques suivantes :  
  
-   [Planifier l’infrastructure du proxy d’application Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Planifier le serveur proxy d’application Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
Pour déployer le proxy d’application Web, vous pouvez suivre les procédures des rubriques suivantes :  
  
-   [Configurer l’infrastructure du proxy d’application Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Installer et configurer le serveur proxy d’application Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>Voir aussi 

[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

