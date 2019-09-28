---
title: Migrer le serveur proxy de fédération AD FS 2,0
description: Contient des informations sur la migration d’un serveur proxy AD FS vers Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 57367cadd3c7ce3d031c6eb3a53c333422543dae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359370"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Migrer le serveur proxy Services ADFS vers Windows Server 2012 R2

Dans Services ADFS (AD FS) dans Windows Server 2012 R2, le rôle de serveur proxy de Fédération est géré par un nouveau service de rôle accès à distance appelé proxy d’application Web. Dans Windows Server 2012 R2, vous pouvez déployer un ou plusieurs proxys d’application Web pour permettre à votre AD FS d’être accessible à partir de l’extérieur du réseau d’entreprise. Toutefois, vous ne pouvez pas migrer un serveur proxy de Fédération exécuté sur Windows Server 2008 R2 ou Windows Server 2012 vers un proxy d’application Web exécuté sur Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  La migration d’un serveur proxy de Fédération exécuté sur Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 vers un proxy d’application Web exécuté sur Windows Server 2012 R2 n’est pas prise en charge.  
  
Si vous souhaitez configurer AD FS dans une batterie de serveurs Windows Server 2012 R2 migrée pour l’accès extranet, vous devez effectuer un nouveau déploiement d’un ou de plusieurs ordinateurs de proxy d’application Web dans le cadre de votre infrastructure AD FS.  
  
Pour planifier le déploiement de proxy d’application Web, vous pouvez passer en revue les informations des rubriques suivantes :  
  
- [Planifier l’infrastructure du proxy d’application Web](https://technet.microsoft.com/library/dn383648.aspx)  
  
- [Planifier le serveur proxy d’application Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
  Pour déployer le proxy d’application Web, vous pouvez suivre les procédures des rubriques suivantes :  
  
- [Configurer l’infrastructure du proxy d’application Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
- [Installer et configurer le serveur proxy d’application Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Étapes suivantes
 [Migrer Services ADFS services de rôle vers Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Préparation de la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Vérification de la migration de AD FS vers Windows Server 2012 R2](verify-ad-fs-migration.md)

