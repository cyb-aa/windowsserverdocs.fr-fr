---
title: Migrer les services de rôle des services de fédération Active Directory (AD FS) vers Windows Server 2012 R2
description: Fournit des instructions pour la migration du service AD FS vers Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6c2f9c8079eb2dfaf208c8835940351a925d0a16
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857502"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Migrer les services de rôle des services de fédération Active Directory (AD FS) vers Windows Server 2012 R2
 Ce document fournit des instructions pour migrer les services de rôle suivants vers Services ADFS (AD FS) qui est installé avec Windows Server 2012 R2 :  
  
-   AD FS serveur de Fédération 2,0 installé sur Windows Server 2008 ou Windows Server 2008 R2  
  
-   AD FS serveur de Fédération installé sur Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Scénarios de migration pris en charge  
 Les instructions de migration de ce guide sont constituées des tâches suivantes :  
  
- Exportation des données de configuration AD FS 2,0 à partir de votre serveur qui exécute Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012  
  
- Exécution d’une mise à niveau sur place du système d’exploitation de ce serveur à partir de Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 vers Windows Server 2012 R2. 
  
- Recréation de la configuration d’AD FS d’origine et restauration des paramètres de service AD FS restants sur ce serveur, qui exécute maintenant le rôle de serveur AD FS qui est installé avec Windows Server 2012 R2.  
  
  Ce guide ne contient pas d’instructions pour la migration d’un serveur qui exécute plusieurs rôles. Si votre serveur exécute plusieurs rôles, il est recommandé de concevoir un processus de migration personnalisé spécifique à votre environnement de serveur, en fonction des informations fournies dans les autres guides de migration des rôles. Des guides de migration concernant des rôles supplémentaires sont disponibles sur le [Portail sur la migration de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Systèmes d’exploitation pris en charge  
 Système d’exploitation du serveur de destination :  
  
 Windows Server 2012 R2 (options d’installation minimale et complète)  
  
 Processeur du serveur de destination :  
  
 x64  
  
|Processeur du serveur source|Système d’exploitation du serveur source|  
|-----------------------------|------------------------------------|  
|x86 ou x64| Windows Server 2008, les options d’installation complète et minimale|  
|x64|Windows Server 2008 R2|  
|x64|Option d’installation minimale de Windows Server 2008 R2|  
|x64|Options d’installation minimale et complète de Windows Server 2012|  
  
> [!NOTE]
> - Les versions des systèmes d’exploitation répertoriées dans le tableau précédent correspondent aux plus anciennes combinaisons de systèmes d’exploitation et de Service Packs prises en charge.  
>   -   Les éditions Foundation, standard, Enterprise et Datacenter du système d’exploitation Windows Server sont prises en charge en tant que serveur source ou de destination.  
>   -   Les migrations entre systèmes d’exploitation physiques et systèmes d’exploitation virtuels sont prises en charge.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Fonctionnalités et services de rôle AD FS pris en charge  
 Le tableau suivant décrit les scénarios de migration des services de rôle AD FS et leurs paramètres respectifs qui sont décrits dans ce guide.  
  
|De|Pour AD FS installé avec Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|AD FS serveur de Fédération 2,0 installé sur Windows Server 2008 ou Windows Server 2008 R2|La migration sur le même serveur est prise en charge. Pour plus d’informations, consultez :<p> [Préparation de la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)<p> [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)|  
|AD FS serveur de Fédération installé sur Windows Server 2012|La migration sur le même serveur est prise en charge.  Pour plus d'informations, consultez :<p> [Préparation de la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)<p> [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparation de la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migration du serveur proxy de fédération AD FS](migrate-fed-server-proxy-r2.md)   
 [Vérification de la migration de AD FS vers Windows Server 2012 R2](verify-ad-fs-migration.md)