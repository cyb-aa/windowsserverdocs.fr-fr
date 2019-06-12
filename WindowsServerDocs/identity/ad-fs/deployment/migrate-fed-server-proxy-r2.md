---
title: Migration du serveur de proxy AD FS 2.0 de fédération
description: Fournit des informations sur la migration d’un serveur proxy AD FS vers Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a2eb6c670e704564bed49486b8950dab96da8a80
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444571"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Migrer le serveur de Proxy Active Directory Federation Services vers Windows Server 2012 R2

Dans Active Directory Federation Services (ADFS) dans Windows Server 2012 R2, le rôle d’un serveur proxy de fédération est géré par un nouveau service de rôle accès à distance appelé Proxy d’Application Web. Dans Windows Server 2012 R2, pour activer AD FS pour l’accessibilité depuis l’extérieur du réseau d’entreprise, vous pouvez déployer un ou plusieurs proxys d’Application Web. Toutefois, vous ne pouvez pas migrer un serveur proxy de fédération en cours d’exécution sur Windows Server 2008 R2 ou Windows Server 2012 vers un Proxy d’Application Web s’exécutant sur Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  La migration d’un serveur proxy de fédération en cours d’exécution sur Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 vers un Proxy d’Application Web s’exécutant sur Windows Server 2012 R2 n’est pas pris en charge.  
  
Si vous souhaitez configurer AD FS dans une batterie de serveurs migré de Windows Server 2012 R2 pour l’accès extranet, vous devez effectuer un nouveau déploiement d’un ou plusieurs ordinateurs de Proxy d’Application Web dans le cadre de votre infrastructure AD FS.  
  
Pour planifier le déploiement de proxy d’application Web, vous pouvez passer en revue les informations des rubriques suivantes :  
  
- [Planifier l’Infrastructure de Proxy d’Application Web](https://technet.microsoft.com/library/dn383648.aspx)  
  
- [Planifier le serveur de Proxy d’Application Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
  Pour déployer le proxy d’application Web, vous pouvez suivre les procédures des rubriques suivantes :  
  
- [Configurer l’Infrastructure de Proxy d’Application Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
- [Installer et configurer le serveur de Proxy d’Application Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Étapes suivantes
 [Migrer des Services de rôle Active Directory Federation Services vers Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Préparation à la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Vérification de la Migration des services AD FS vers Windows Server 2012 R2](verify-ad-fs-migration.md)

