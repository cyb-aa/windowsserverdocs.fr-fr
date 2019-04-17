---
title: "Migrer le rôle Services de fédération Active Directory pour Windows Server 2012 R2"
description: Fournit des instructions pour la migration du service AD FS vers Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 478729a7b6560beba5f04a1a15ad035561ad31f0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Migrer le rôle Services de fédération Active Directory pour Windows Server 2012 R2
 Ce document fournit des instructions pour migrer les services de rôle suivants vers Active Directory Federation Services (ADFS) qui est installé avec Windows Server 2012 R2:  
  
-   Serveur de fédération AD FS 2.0 installé sur Windows Server 2008 ou Windows Server 2008 R2  
  
-   Serveur de fédération AD FS installé sur Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Scénarios de migration pris en charge  
 Les instructions de migration dans ce guide sont constituées des tâches suivantes:  
  
-   Exportation des données AD FS 2.0 de configuration de votre serveur qui exécute Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012  
  
-   Mise à niveau sur place du système d’exploitation de ce serveur à partir de Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 vers Windows Server 2012 R2. 
  
-   Recréation de la configuration AD FS d’origine et restauration AD FS restants paramètres de service sur ce serveur, qui s’exécute maintenant le rôle de serveur AD FS qui est installé avec Windows Server 2012 R2.  
  
 Ce guide n’inclut pas d’instructions pour migrer un serveur qui exécute plusieurs rôles. Si votre serveur exécute plusieurs rôles, nous vous recommandons de concevoir un processus de migration personnalisée spécifique à votre environnement de serveur, basée sur les informations fournies dans les autres guides de migration de rôle. Guides de migration des rôles supplémentaires sont disponibles sur le [portail de Migration de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Systèmes d’exploitation pris en charge  
 Système d’exploitation de serveur destination:  
  
 Windows Server 2012 R2 (options d’installation complète et minimale)  
  
 Processeur du serveur de destination:  
  
 x64 64  
  
|Processeur du serveur source|Système d’exploitation du serveur source|  
|-----------------------------|------------------------------------|  
|x86-ou x 64-en fonction| Windows Server 2008, complète et minimale|  
|x64 64|Windows Server2008R2|  
|x64 64|Option d’installation minimale de Windows Server 2008 R2|  
|x64 64|Server Core et options d’installation complète de Windows Server 2012|  
  
> [!NOTE]
>  -   Les versions des systèmes d’exploitation répertoriés dans le tableau précédent sont aux plus anciennes combinaisons de systèmes d’exploitation et les service packs pris en charge.  
> -   Les éditions Foundation, Standard, Enterprise et Datacenter du système d’exploitation Windows Server sont prises en charge en tant que la source ou le serveur de destination.  
> -   Les migrations entre systèmes d’exploitation physiques et systèmes d’exploitation virtuels sont prises en charge.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Prise en charge des services de rôle AD FS et des fonctionnalités  
 Le tableau suivant décrit les scénarios de migration des services de rôle AD FS et leurs paramètres respectifs qui sont décrites dans ce guide.  
  
|De|Vers AD FS installé avec Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|Serveur de fédération AD FS 2.0 installé sur Windows Server 2008 ou Windows Server 2008 R2|Migration sur le même serveur est prise en charge. Pour plus d’informations, voir:<br /><br /> [Préparation de la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)|  
|Serveur de fédération AD FS installé sur Windows Server 2012|Migration sur le même serveur est prise en charge.  Pour plus d’informations, voir:<br /><br /> [Préparation de la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparation de la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migration du serveur Proxy de fédération AD FS](migrate-fed-server-proxy-r2.md)   
 [Vérification de la Migration AD FS Windows Server 2012 R2](verify-ad-fs-migration.md)