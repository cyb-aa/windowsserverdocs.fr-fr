---
title: "Migration du serveur de proxy AD FS 2.0 de fédération"
description: "Fournit des informations sur la migration d’un serveur de proxy AD FS vers Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 33ab29fd5efdb0bdd1fe25580e3f4434071e1c7d
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2017
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Migrer le serveur Proxy de Active Directory Federation Services pour Windows Server 2012 R2

Dans Active Directory Federation Services (ADFS) dans Windows Server 2012 R2, le rôle d’un serveur proxy de fédération est géré par un nouveau service de rôle accès à distance appelé Proxy d’Application Web. Dans Windows Server 2012 R2, pour activer AD FS pour l’accessibilité depuis l’extérieur du réseau d’entreprise, vous pouvez déployer un ou plusieurs proxys d’Application Web. Toutefois, vous ne pouvez pas migrer un serveur proxy de fédération exécuté sous Windows Server 2008 R2 ou Windows Server 2012 vers un Proxy d’Application Web s’exécutant sur Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  La migration d’un serveur proxy de fédération exécuté sous Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 vers un Proxy d’Application Web s’exécutant sur Windows Server 2012 R2 n’est pas pris en charge.  
  
Si vous voulez configurer AD FS dans une batterie migrée de Windows Server 2012 R2 pour l’accès extranet, vous devez effectuer un nouveau déploiement d’un ou plusieurs ordinateurs de Proxy d’Application Web dans le cadre de votre infrastructure AD FS.  
  
Pour planifier le déploiement de Proxy d’Application Web, vous pouvez passer en revue les informations contenues dans les rubriques suivantes:  
  
-   [Planifier l’Infrastructure du Proxy d’Application Web](https://technet.microsoft.com/en-us/library/dn383648.aspx)  
  
-   [Planifier le serveur de Proxy d’Application Web](https://technet.microsoft.com/en-us/library/dn383647.aspx)  
  
 Pour déployer le proxy d’Application Web, vous pouvez suivre les procédures décrites dans les rubriques suivantes:  
  
-   [Configurer l’Infrastructure du Proxy d’Application Web](https://technet.microsoft.com/en-us/library/dn383644.aspx)  
  
-   [Installer et configurer le serveur de Proxy d’Application Web](https://technet.microsoft.com/en-us/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Étapes suivantes
 [Migrer le rôle Services de fédération Active Directory pour Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Préparation de la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Vérification de la Migration AD FS Windows Server 2012 R2](verify-ad-fs-migration.md)

