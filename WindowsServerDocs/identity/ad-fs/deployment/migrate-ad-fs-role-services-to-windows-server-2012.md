---
title: "Migrer le rôle Services de fédération Active Directory pour Windows Server 2012"
description: Fournit des instructions pour la migration du service AD FS vers Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b44ed504c2b3dc8a8ac0ca9648f1db9e362e075
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Migrer le rôle Services de fédération Active Directory pour Windows Server 2012

La liste suivante fournit des instructions sur la migration des services de rôle suivants vers Active Directory Federation Services (ADFS) sur Windows Server 2012:  
  
-   Agent de base de jetons AD FS 1.1 Windows et les services AD FS 1.1 prenant en charge les revendications agent est installé avec Windows Server 2008 ou Windows Server 2008 R2  
  
-   Serveur de fédération AD FS 2.0 et AD FS serveur proxy de fédération 2.0 installé sur Windows Server 2008 ou Windows Server 2008 R2    
  
## <a name="supported-migration-scenarios"></a>Scénarios de migration pris en charge  
 Les instructions de migration contient les tâches suivantes:  
  
-   Exportation des données de configuration 2.0 AD FS à partir de votre serveur qui exécute Windows Server 2008 ou Windows Server 2008 R2  
  
-   Mise à niveau sur place du système d’exploitation de ce serveur à partir de Windows Server 2008 ou Windows Server 2008 R2 vers Windows Server 2012.
  
-   Recréation de la configuration AD FS d’origine et restauration AD FS restants paramètres de service sur ce serveur, qui s’exécute maintenant le rôle de serveur AD FS qui est installé avec Windows Server 2012.  
  
 Ce guide n’inclut pas d’instructions pour migrer un serveur qui exécute plusieurs rôles. Si votre serveur exécute plusieurs rôles, nous vous recommandons de concevoir un processus de migration personnalisée spécifique à votre environnement de serveur, basée sur les informations fournies dans les autres guides de migration de rôle. Guides de migration des rôles supplémentaires sont disponibles sur le [portail de Migration de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
## <a name="supported-operating-systems"></a>Systèmes d’exploitation pris en charge  
 **Système d’exploitation de serveur destination:**  
  

-  Windows Server 2012 ou Windows Server 2008 R2 (options d’installation complète et minimale)  
  
 **Processeur du serveur de destination:**  
  

-  x64 64  
  
|Processeur du serveur source|Système d’exploitation du serveur source|  
|-----|-----|  
|x86-ou x 64-en fonction|Windows Server 2003 avec Service Pack 2|  
|x86-ou x 64-en fonction|Windows Server 2003 R2|  
|x86-ou x 64-en fonction|Windows Server 2008, complète et minimale|  
|x64 64|Windows Server2008R2|  
|x64 64|Option d’installation minimale de Windows Server 2008 R2|  
|x64 64|Server Core et options d’installation complète de Windows Server 2012|  
  
> [!NOTE]
>  -   Les versions des systèmes d’exploitation répertoriés dans le tableau précédent sont aux plus anciennes combinaisons de systèmes d’exploitation et les service packs pris en charge.  
> -   Les éditions Foundation, Standard, Enterprise et Datacenter du système d’exploitation Windows Server sont prises en charge en tant que la source ou le serveur de destination.  
> -   Les migrations entre systèmes d’exploitation physiques et systèmes d’exploitation virtuels sont prises en charge.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Prise en charge des services de rôle AD FS et des fonctionnalités  
 Le tableau suivant décrit les scénarios de migration des services de rôle AD FS et leurs paramètres respectifs qui sont décrites dans ce guide.  
  
|De|Pour AD FS installé avec Windows Server 2012|  
|----------|-----|  
|Serveur de fédération AD FS 1.0 installé avec Windows Server 2003 R2|La migration n’est pas pris en charge.|  
|Proxy de serveur AD FS 1.0 fédération installé avec Windows Server 2003 R2|La migration n’est pas pris en charge.|  
|Agent AD FS 1.0 Windows basée sur le jeton installé avec Windows Server 2003 R2|La migration n’est pas pris en charge.|  
|AD FS 1.0 prenant en charge les revendications agent installé avec Windows Server 2003 R2)|La migration n’est pas pris en charge.|  
|Serveur de fédération AD FS 1.1 installé avec Windows Server 2008 ou Windows Server 2008 R2|La migration n’est pas pris en charge.|  
|Proxy de serveur AD FS 1.1 fédération installé avec Windows Server 2008 ou Windows Server 2008 R2|La migration n’est pas pris en charge.|  
|Agent AD FS 1.1 Windows basée sur le jeton installé avec Windows Server 2008 ou Windows Server 2008 R2|Migration sur le même serveur est prise en charge, mais l’agent de basée sur les jetons Windows AD FS migrée fonctionne qu’avec un service de fédération 1.1 AD FS installé avec Windows Server 2008 ou Windows Server 2008 R2. Pour plus d’informations, voir:<br /><br /> [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)<br /><br /> [Il interagit avec AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS 1.1 prenant en charge les revendications agent installé avec Windows Server 2008 ou Windows Server 2008 R2)|Migration sur le même serveur est prise en charge. L’agent de 1.1 web prenant en charge les revendications AD FS migré fonctionne avec les éléments suivants:<br /><br /> Service de fédération AD FS 1.1 installé avec Windows Server 2008 ou Windows Server 2008 R2<br /><br /> Service de 2.0 de fédération AD FS installé sur Windows Server 2008 ou Windows Server 2008 R2<br /><br /> Service de fédération AD FS installé avec Windows Server 2012<br /><br /> Pour plus d’informations, voir:<br /><br /> [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)<br /><br /> [Il interagit avec AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|Serveur de fédération AD FS 2.0 installé sur Windows Server 2008 ou Windows Server 2008 R2|Migration sur le même serveur est prise en charge. Pour plus d’informations, voir:<br /><br /> [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)|  
|Proxy de serveur AD FS 2.0 fédération installé sur Windows Server 2008 ou Windows Server 2008 R2|Migration sur le même serveur est prise en charge.  Pour plus d’informations, voir:<br /><br /> [Préparer la migration AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)