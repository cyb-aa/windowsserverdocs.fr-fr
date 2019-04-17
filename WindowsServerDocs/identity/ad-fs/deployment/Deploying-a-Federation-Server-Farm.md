---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: "Guide de déploiement de Windows Server2012R2 ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f1ea6830237813e6fd2bd6a172f467e8d81065
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="deploying-a-federation-server-farm"></a>Déploiement d’une batterie de serveurs de fédération

>S’applique à: Windows Server2016, Windows Server2012R2

Pour déployer une batterie de serveurs de fédération, effectuez les tâches de cette liste de vérification dans l’ordre. Lorsqu’un lien de référence vous redirige vers une rubrique conceptuelle, revenez à cette liste de vérification une fois que vous avez consulté la rubrique conceptuelle afin de pouvoir effectuer les tâches restantes de cette liste de vérification.  
  
![Déploiement de la batterie de serveurs fédérés](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification: déploiement d’une batterie de serveurs de fédération**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![Déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Passez en revue les concepts importants et les considérations de lorsque vous vous préparez à déployer ActiveDirectory Federation Services \(ADFS\). **Remarque:**|![Déploiement de la batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Guide de conception ADFS dans Windows Server2012R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<br /><br />![Déploiement de la batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Understanding Key ADFS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Si vous décidez d’utiliser MicrosoftSQLServer pour votre magasin de configuration ADFS, prenez soin de déployer une instance fonctionnelle de SQLServer.|[SQLServer](https://technet.microsoft.com/sqlserver)**Avertissement:** dans Windows Server2012R2, si vous souhaitez créer une batterie ADFS et utiliser SQLServer pour stocker vos données de configuration, vous pouvez utiliser SQLServer2008 et versions plus récentes, notamment SQLServer2012.|  
|![Déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Joignez votre ordinateur à un domaine ActiveDirectory.|![Déploiement de la batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[joindre un ordinateur à un domaine](Join-a-Computer-to-a-Domain.md)|  
|![Déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Inscrire un certificat Secure Socket Layer \(SSL\) pour ADFS.|![Déploiement de la batterie de serveurs fédérés](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[inscrire un certificat SSL pour ADFS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![Déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Installer le service de rôle ADFS.|![Déploiement de la batterie de serveurs fédérés](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[installer le Service de rôle ADFS](Install-the-AD-FS-Role-Service.md)|  
|![Déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Configurer un serveur de fédération.|![Déploiement de la batterie de serveurs fédérés](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[configurer un serveur de fédération](Configure-a-Federation-Server.md)|  
|![Déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Étape facultative: configurer un serveur de fédération avec Device Registration Service \(DRS\).|![Déploiement de la batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurer un serveur de fédération avec Device Registration Service](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![Déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Ajouter un enregistrement de ressource \(CNAME\) \(A\) et alias hôte d’entreprise \(DNS\) système de nom de domaine pour le service de fédération et DRS.|![Déploiement de la batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurer le DNS d’entreprise pour le Service de fédération et DRS](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![Déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Vérifiez qu’un serveur de fédération est opérationnel.|![Déploiement de la batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Vérifiez qu’une fédération serveur est opérationnel](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Voir aussi  
[Déploiement d’ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server2012R2 ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

