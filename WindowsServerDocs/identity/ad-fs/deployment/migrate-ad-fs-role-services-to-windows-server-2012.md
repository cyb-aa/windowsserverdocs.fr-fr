---
title: Migrer les services de rôle des services AD FS (Active Directory Federation Services) vers Windows Server 2012
description: Fournit des instructions sur la migration du service AD FS vers Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 63dc9825b9c9b8a06d0946859e08a536589eb044
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445702"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Migrer les services de rôle des services AD FS (Active Directory Federation Services) vers Windows Server 2012

Ce qui suit fournit des instructions sur la migration des services de rôle suivants vers Active Directory Federation Services (ADFS) sur Windows Server 2012 :  
  
-   L’agent AD FS 1.1 Windows basée sur les jetons et AD FS 1.1 prenant en charge les revendications agent est installé avec Windows Server 2008 ou Windows Server 2008 R2  
  
-   Serveur de fédération AD FS 2.0 et AD FS serveur proxy de fédération 2.0 installé sur Windows Server 2008 ou Windows Server 2008 R2    
  
## <a name="supported-migration-scenarios"></a>Scénarios de migration pris en charge  
 Les instructions de migration contient les tâches suivantes :  
  
- Exportation des données de configuration 2.0 AD FS à partir de votre serveur qui exécute Windows Server 2008 ou Windows Server 2008 R2  
  
- Effectuer une mise à niveau sur place du système d’exploitation de ce serveur à partir de Windows Server 2008 ou Windows Server 2008 R2 vers Windows Server 2012.
  
- Recréation de la configuration AD FS d’origine et restauration d’AD FS restants paramètres de service sur ce serveur, qui s’exécute maintenant le rôle de serveur AD FS qui est installé avec Windows Server 2012.  
  
  Ce guide ne contient pas d’instructions pour la migration d’un serveur qui exécute plusieurs rôles. Si votre serveur exécute plusieurs rôles, il est recommandé de concevoir un processus de migration personnalisé spécifique à votre environnement de serveur, en fonction des informations fournies dans les autres guides de migration des rôles. Des guides de migration concernant des rôles supplémentaires sont disponibles sur le [Portail sur la migration de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
## <a name="supported-operating-systems"></a>Systèmes d’exploitation pris en charge  
 **Système de d’exploitation du serveur de destination :**  
  

- Windows Server 2012 ou Windows Server 2008 R2 (options d’installation complète et minimale)  
  
  **Processeur du serveur de destination :**  
  

- x64  
  
|Processeur du serveur source|Système d’exploitation du serveur source|  
|-----|-----|  
|x86 ou x64|Windows Server 2003 avec Service Pack 2|  
|x86 ou x64|Windows Server 2003 R2|  
|x86 ou x64|Windows Server 2008, installations complète et les options d’installation Server Core|  
|x64|Windows Server 2008 R2|  
|x64|Option d’installation Server Core de Windows Server 2008 R2|  
|x64|Server Core et les options d’installation complète de Windows Server 2012|  
  
> [!NOTE]
> - Les versions des systèmes d’exploitation répertoriées dans le tableau précédent correspondent aux plus anciennes combinaisons de systèmes d’exploitation et de Service Packs prises en charge.  
>   -   Les éditions Foundation, Standard, Enterprise et Datacenter du système d’exploitation Windows Server sont prises en charge en tant que la source ou le serveur de destination.  
>   -   Les migrations entre systèmes d’exploitation physiques et systèmes d’exploitation virtuels sont prises en charge.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Services de rôle AD FS et les fonctionnalités prises en charge  
 Le tableau suivant décrit les scénarios de migration des services de rôle AD FS et leurs paramètres respectifs qui sont décrites dans ce guide.  
  
|De|Pour AD FS installé avec Windows Server 2012|  
|----------|-----|  
|Serveur de fédération AD FS 1.0 installé avec Windows Server 2003 R2|La migration n'est pas prise en charge.|  
|Proxy de serveur AD FS 1.0 fédération installé avec Windows Server 2003 R2|La migration n'est pas prise en charge.|  
|AD FS 1.0 Windows basée sur JETON l’agent installé avec Windows Server 2003 R2|La migration n'est pas prise en charge.|  
|AD FS 1.0 prenant en charge les revendications agent installé avec Windows Server 2003 R2)|La migration n'est pas prise en charge.|  
|Serveur de fédération AD FS 1.1 installé avec Windows Server 2008 ou Windows Server 2008 R2|La migration n'est pas prise en charge.|  
|Proxy de serveur AD FS 1.1 fédération installé avec Windows Server 2008 ou Windows Server 2008 R2|La migration n'est pas prise en charge.|  
|AD FS 1.1 Windows basée sur JETON l’agent installé avec Windows Server 2008 ou Windows Server 2008 R2|Migration sur le même serveur est prise en charge, mais l’agent de basée sur les jetons AD FS Windows migré fonctionne qu’avec un service de fédération 1.1 AD FS installé avec Windows Server 2008 ou Windows Server 2008 R2. Pour plus d'informations, consultez :<br /><br /> [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interaction avec AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS 1.1 prenant en charge les revendications agent installé avec Windows Server 2008 ou Windows Server 2008 R2)|La migration sur le même serveur est prise en charge. L’agent de 1.1 web prenant en charge les revendications AD FS migrée fonctionnera avec les éléments suivants :<br /><br /> Service de fédération AD FS 1.1 installé avec Windows Server 2008 ou Windows Server 2008 R2<br /><br /> AD FS 2.0 federation service est installé sur Windows Server 2008 ou Windows Server 2008 R2<br /><br /> Service de fédération AD FS installé avec Windows Server 2012<br /><br /> Pour plus d'informations, consultez :<br /><br /> [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interaction avec AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|Serveur de fédération AD FS 2.0 installé sur Windows Server 2008 ou Windows Server 2008 R2|La migration sur le même serveur est prise en charge. Pour plus d'informations, consultez :<br /><br /> [Préparer la migration du serveur de fédération AD FS 2.0](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [Migrer le serveur de fédération AD FS 2.0](migrate-the-ad-fs-fed-server.md)|  
|Proxy de serveur AD FS 2.0 federation installé sur Windows Server 2008 ou Windows Server 2008 R2|La migration sur le même serveur est prise en charge.  Pour plus d’informations, voir :<br /><br /> [Préparer la migration du serveur proxy de fédération AD FS 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [Migrer le serveur proxy de fédération AD FS 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur Proxy pour AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)