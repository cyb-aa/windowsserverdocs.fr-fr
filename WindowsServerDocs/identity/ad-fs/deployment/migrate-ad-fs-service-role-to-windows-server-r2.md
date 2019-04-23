---
title: Migrer les services de rôle des services de fédération Active Directory (AD FS) vers Windows Server 2012 R2
description: Fournit des instructions sur la migration du service AD FS vers Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 478729a7b6560beba5f04a1a15ad035561ad31f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847950"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Migrer les services de rôle des services de fédération Active Directory (AD FS) vers Windows Server 2012 R2
 Ce document fournit des instructions pour migrer les services de rôle suivants vers Active Directory Federation Services (ADFS) qui est installé avec Windows Server 2012 R2 :  
  
-   Serveur de fédération AD FS 2.0 installé sur Windows Server 2008 ou Windows Server 2008 R2  
  
-   Serveur de fédération AD FS installé sur Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Scénarios de migration pris en charge  
 Les instructions de migration de ce guide sont constituées des tâches suivantes :  
  
-   Exportation des données de configuration 2.0 AD FS à partir de votre serveur qui exécute Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012  
  
-   Effectuer une mise à niveau sur place du système d’exploitation de ce serveur à partir de Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 vers Windows Server 2012 R2. 
  
-   Recréation de la configuration AD FS d’origine et restauration d’AD FS restants paramètres de service sur ce serveur, qui s’exécute maintenant le rôle de serveur AD FS qui est installé avec Windows Server 2012 R2.  
  
 Ce guide ne contient pas d’instructions pour la migration d’un serveur qui exécute plusieurs rôles. Si votre serveur exécute plusieurs rôles, il est recommandé de concevoir un processus de migration personnalisé spécifique à votre environnement de serveur, en fonction des informations fournies dans les autres guides de migration des rôles. Des guides de migration concernant des rôles supplémentaires sont disponibles sur le [Portail sur la migration de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Systèmes d’exploitation pris en charge  
 Système de d’exploitation du serveur de destination :  
  
 Windows Server 2012 R2 (options d’installation complète et minimale)  
  
 Processeur du serveur de destination :  
  
 x64  
  
|Processeur du serveur source|Système d’exploitation du serveur source|  
|-----------------------------|------------------------------------|  
|x86 ou x64| Windows Server 2008, installations complète et les options d’installation Server Core|  
|x64|Windows Server 2008 R2|  
|x64|Option d’installation Server Core de Windows Server 2008 R2|  
|x64|Server Core et les options d’installation complète de Windows Server 2012|  
  
> [!NOTE]
>  -   Les versions des systèmes d’exploitation répertoriées dans le tableau précédent correspondent aux plus anciennes combinaisons de systèmes d’exploitation et de Service Packs prises en charge.  
> -   Les éditions Foundation, Standard, Enterprise et Datacenter du système d’exploitation Windows Server sont prises en charge en tant que la source ou le serveur de destination.  
> -   Les migrations entre systèmes d’exploitation physiques et systèmes d’exploitation virtuels sont prises en charge.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Services de rôle AD FS et les fonctionnalités prises en charge  
 Le tableau suivant décrit les scénarios de migration des services de rôle AD FS et leurs paramètres respectifs qui sont décrites dans ce guide.  
  
|De|Pour AD FS installé avec Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|Serveur de fédération AD FS 2.0 installé sur Windows Server 2008 ou Windows Server 2008 R2|La migration sur le même serveur est prise en charge. Pour plus d'informations, consultez :<br /><br /> [Préparation à la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)|  
|Serveur de fédération AD FS installé sur Windows Server 2012|La migration sur le même serveur est prise en charge.  Pour plus d’informations, voir :<br /><br /> [Préparation à la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparation à la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migration de serveur Proxy de fédération AD FS](migrate-fed-server-proxy-r2.md)   
 [Vérification de la Migration des services AD FS vers Windows Server 2012 R2](verify-ad-fs-migration.md)