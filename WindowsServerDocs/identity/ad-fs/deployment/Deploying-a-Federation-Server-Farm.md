---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Guide de déploiement des services AD FS Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e5507cd567114d17c6500655ee210b70bd9ea1ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408412"
---
# <a name="deploying-a-federation-server-farm"></a>Déploiement d’une batterie de serveurs de fédération


Pour déployer une batterie de serveurs de fédération, effectuez les tâches de cette liste de vérification dans l’ordre. Lorsqu’un lien de référence vous redirige vers une rubrique conceptuelle, retournez dans cette liste de vérification une fois que vous avez consulté la rubrique conceptuelle afin de pouvoir effectuer les tâches restantes.  
  
![deploying-batterie de serveurs fédérés @ no__t-1Checklist : Déploiement d’une batterie de serveurs de Fédération @ no__t-0  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Passez en revue les concepts importants et les considérations à prendre en compte lors de la préparation du déploiement de Services ADFS \(AD FS @ no__t-1. **Remarque :**|](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS Guide de conception de la batterie de serveurs fédérés @no__t 0deploying dans Windows server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />@no__t 0deploying-batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[comprenant les concepts clés AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Si vous décidez d’utiliser Microsoft SQL Server pour votre magasin de configuration AD FS, prenez soin de déployer une instance fonctionnelle de SQL Server.|[SQL Server](https://technet.microsoft.com/sqlserver) **Avertissement :** Dans Windows Server 2012 R2, si vous voulez créer une batterie AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et ses versions plus récentes, notamment SQL Server 2012.|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Joignez votre ordinateur à un domaine Active Directory.|![deploying batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[joindre un ordinateur à un domaine](Join-a-Computer-to-a-Domain.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Inscrire un certificat Secure Socket Layer \(SSL @ no__t-1 pour AD FS.|![deploying batterie de serveurs fédérés](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[inscrire un certificat SSL pour AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Installez le service de rôle AD FS.|![deploying batterie de serveurs fédérés](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[installer le service de rôle AD FS](Install-the-AD-FS-Role-Service.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Configurez un serveur de fédération.|![deploying batterie de serveurs fédérés](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[configurer un serveur de Fédération](Configure-a-Federation-Server.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Étape facultative : Configurez un serveur de Fédération avec Device Registration service \(DRS @ no__t-1.|![deploying batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurer un serveur de Fédération avec Device Registration service](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Ajoutez un hôte \(A @ no__t-1 et un alias \(CNAME @ no__t-3 à un enregistrement de ressource de nom de domaine d’entreprise \(DNS @ no__t-5 pour le service de Fédération et DRS.|la batterie de serveurs fédérés @no__t 0deploying](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configure le DNS d’entreprise pour les service FS (Federation Service) et Drs](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Vérifiez qu’un serveur de fédération est opérationnel.|![deploying batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[vérifier qu’un serveur de Fédération est opérationnel](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Voir aussi  
[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

