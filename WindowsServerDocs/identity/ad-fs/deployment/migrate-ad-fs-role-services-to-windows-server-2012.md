---
title: Migrer les services de rôle des services AD FS (Active Directory Federation Services) vers Windows Server 2012
description: Fournit des instructions pour la migration du service AD FS vers Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cdb5523ade5c3c7572656d62d1b4f744683ec96e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408270"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Migrer les services de rôle des services AD FS (Active Directory Federation Services) vers Windows Server 2012

La rubrique suivante fournit des instructions sur la migration des services de rôle suivants vers Services ADFS (AD FS) sur Windows Server 2012 :  
  
-   Agent basé sur les jetons Windows AD FS 1,1 et agent AD FS 1,1 prenant en charge les revendications installé avec Windows Server 2008 ou Windows Server 2008 R2  
  
-   AD FS serveur de Fédération 2,0 et AD FS serveur proxy de Fédération 2,0 installés sur Windows Server 2008 ou Windows Server 2008 R2    
  
## <a name="supported-migration-scenarios"></a>Scénarios de migration pris en charge  
 Les instructions de migration contiennent les tâches suivantes :  
  
- Exportation des données de configuration AD FS 2,0 à partir de votre serveur qui exécute Windows Server 2008 ou Windows Server 2008 R2  
  
- Exécution d’une mise à niveau sur place du système d’exploitation de ce serveur à partir de Windows Server 2008 ou Windows Server 2008 R2 vers Windows Server 2012.
  
- Recréation de la configuration d’AD FS d’origine et restauration des paramètres de service AD FS restants sur ce serveur, qui exécute maintenant le rôle de serveur AD FS qui est installé avec Windows Server 2012.  
  
  Ce guide ne contient pas d’instructions pour la migration d’un serveur qui exécute plusieurs rôles. Si votre serveur exécute plusieurs rôles, il est recommandé de concevoir un processus de migration personnalisé spécifique à votre environnement de serveur, en fonction des informations fournies dans les autres guides de migration des rôles. Des guides de migration concernant des rôles supplémentaires sont disponibles sur le [Portail sur la migration de Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
## <a name="supported-operating-systems"></a>Systèmes d’exploitation pris en charge  
 **Système d’exploitation du serveur de destination :**  
  

- Windows Server 2012 ou Windows Server 2008 R2 (options d’installation minimale et complète)  
  
  **Processeur du serveur de destination :**  
  

- x64  
  
|Processeur du serveur source|Système d’exploitation du serveur source|  
|-----|-----|  
|x86 ou x64|Windows Server 2003 avec Service Pack 2|  
|x86 ou x64|Windows Server 2003 R2|  
|x86 ou x64|Windows Server 2008, les options d’installation complète et minimale|  
|x64|Windows Server 2008 R2|  
|x64|Option d’installation minimale de Windows Server 2008 R2|  
|x64|Options d’installation minimale et complète de Windows Server 2012|  
  
> [!NOTE]
> - Les versions des systèmes d’exploitation répertoriées dans le tableau précédent correspondent aux plus anciennes combinaisons de systèmes d’exploitation et de Service Packs prises en charge.  
>   -   Les éditions Foundation, standard, Enterprise et Datacenter du système d’exploitation Windows Server sont prises en charge en tant que serveur source ou de destination.  
>   -   Les migrations entre systèmes d’exploitation physiques et systèmes d’exploitation virtuels sont prises en charge.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Fonctionnalités et services de rôle AD FS pris en charge  
 Le tableau suivant décrit les scénarios de migration des services de rôle AD FS et leurs paramètres respectifs qui sont décrits dans ce guide.  
  
|De|Pour AD FS installé avec Windows Server 2012|  
|----------|-----|  
|AD FS serveur de Fédération 1,0 installé avec Windows Server 2003 R2|La migration n'est pas prise en charge.|  
|AD FS 1,0 proxy de serveur de Fédération installé avec Windows Server 2003 R2|La migration n'est pas prise en charge.|  
|Agent basé sur les jetons Windows AD FS 1,0 installé avec Windows Server 2003 R2|La migration n'est pas prise en charge.|  
|AD FS 1,0 agent prenant en charge les revendications installé avec Windows Server 2003 R2)|La migration n'est pas prise en charge.|  
|AD FS serveur de Fédération 1,1 installé avec Windows Server 2008 ou Windows Server 2008 R2|La migration n'est pas prise en charge.|  
|AD FS 1,1 proxy de serveur de Fédération installé avec Windows Server 2008 ou Windows Server 2008 R2|La migration n'est pas prise en charge.|  
|Agent basé sur les jetons Windows AD FS 1,1 installé avec Windows Server 2008 ou Windows Server 2008 R2|La migration sur le même serveur est prise en charge, mais l’agent à base de jetons Windows AD FS migré fonctionne uniquement avec un service de fédération AD FS 1,1 installé avec Windows Server 2008 ou Windows Server 2008 R2. Pour plus d’informations, consultez :<br /><br /> [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interaction avec AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS 1,1 agent prenant en charge les revendications installé avec Windows Server 2008 ou Windows Server 2008 R2)|La migration sur le même serveur est prise en charge. L’agent Web prenant en charge les revendications AD FS 1,1 migré fonctionne avec les éléments suivants :<br /><br /> Service de fédération AD FS 1,1 installé avec Windows Server 2008 ou Windows Server 2008 R2<br /><br /> Service de fédération AD FS 2,0 installé sur Windows Server 2008 ou Windows Server 2008 R2<br /><br /> Service de fédération AD FS installé avec Windows Server 2012<br /><br /> Pour plus d’informations, consultez :<br /><br /> [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interaction avec AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS serveur de Fédération 2,0 installé sur Windows Server 2008 ou Windows Server 2008 R2|La migration sur le même serveur est prise en charge. Pour plus d’informations, consultez :<br /><br /> [Préparer la migration du serveur de fédération AD FS 2.0](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [Migrer le serveur de fédération AD FS 2.0](migrate-the-ad-fs-fed-server.md)|  
|AD FS 2,0 proxy de serveur de Fédération installé sur Windows Server 2008 ou Windows Server 2008 R2|La migration sur le même serveur est prise en charge.  Pour plus d’informations, voir :<br /><br /> [Préparer la migration du serveur proxy de fédération AD FS 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [Migrer le serveur proxy de fédération AD FS 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur proxy de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur de fédération AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur proxy de fédération AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)