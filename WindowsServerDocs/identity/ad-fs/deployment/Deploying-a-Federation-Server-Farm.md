---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Guide de déploiement des services AD FS Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c0f8dca425f644952c36a289ec72651f6383e846
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192188"
---
# <a name="deploying-a-federation-server-farm"></a>Déploiement d’une batterie de serveurs de fédération


Pour déployer une batterie de serveurs de fédération, effectuez les tâches de cette liste de vérification dans l’ordre. Lorsqu’un lien de référence vous redirige vers une rubrique conceptuelle, retournez dans cette liste de vérification une fois que vous avez consulté la rubrique conceptuelle afin de pouvoir effectuer les tâches restantes.  
  
![déploiement de batterie de serveurs fédérés](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification : Déploiement d’une batterie de serveurs de fédération**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![déploiement de batterie de serveurs fédérés](media/icon_checkboxo.gif)|Passez en revue des concepts importants et les considérations relatives à la préparation du déploiement d’Active Directory Federation Services \(AD FS\). **Remarque :**|![déploiement de batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Guide de conception AD FS dans Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />![déploiement de batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Si vous décidez d’utiliser Microsoft SQL Server pour votre magasin de configuration AD FS, prenez soin de déployer une instance fonctionnelle de SQL Server.|[SQL Server](https://technet.microsoft.com/sqlserver) **Avertissement :** Dans Windows Server 2012 R2, si vous voulez créer une batterie AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et ses versions plus récentes, notamment SQL Server 2012.|  
|![déploiement de batterie de serveurs fédérés](media/icon_checkboxo.gif)|Joignez votre ordinateur à un domaine Active Directory.|![déploiement de batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[joindre un ordinateur à un domaine](Join-a-Computer-to-a-Domain.md)|  
|![déploiement de batterie de serveurs fédérés](media/icon_checkboxo.gif)|Inscrire un Secure Socket Layer \(SSL\) certificat pour AD FS.|![déploiement de batterie de serveurs fédérés](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[inscrire un certificat SSL pour AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![déploiement de batterie de serveurs fédérés](media/icon_checkboxo.gif)|Installez le service de rôle AD FS.|![déploiement de batterie de serveurs fédérés](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[installer le Service de rôle AD FS](Install-the-AD-FS-Role-Service.md)|  
|![déploiement de batterie de serveurs fédérés](media/icon_checkboxo.gif)|Configurez un serveur de fédération.|![déploiement de batterie de serveurs fédérés](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[configurer un serveur de fédération](Configure-a-Federation-Server.md)|  
|![déploiement de batterie de serveurs fédérés](media/icon_checkboxo.gif)|Étape facultative : Configurer un serveur de fédération avec Device Registration Service \(DRS\).|![déploiement de batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurer un serveur de fédération avec Device Registration Service](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![déploiement de batterie de serveurs fédérés](media/icon_checkboxo.gif)|Ajouter un hôte \(A\) et alias \(CNAME\) enregistrement de ressource pour le système de nom de domaine d’entreprise \(DNS\) pour le service de fédération et DRS.|![déploiement de batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurer d’entreprise des serveurs DNS pour le Service de fédération et DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![déploiement de batterie de serveurs fédérés](media/icon_checkboxo.gif)|Vérifiez qu’un serveur de fédération est opérationnel.|![déploiement de batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Vérifiez qu’une fédération de serveur est opérationnel](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Voir aussi  
[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

