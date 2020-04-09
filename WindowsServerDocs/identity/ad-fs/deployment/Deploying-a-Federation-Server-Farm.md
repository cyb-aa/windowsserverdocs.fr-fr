---
ms.assetid: bbb5b68f-00ad-4715-8176-0c2769b706c4
title: Guide de déploiement des services AD FS Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bb4d13d13771d76a306a32988c0faa03dd01db49
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855462"
---
# <a name="deploying-a-federation-server-farm"></a>Déploiement d’une batterie de serveurs de fédération


Pour déployer une batterie de serveurs de fédération, effectuez les tâches de cette liste de vérification dans l’ordre. Lorsqu’un lien de référence vous redirige vers une rubrique conceptuelle, retournez dans cette liste de vérification une fois que vous avez consulté la rubrique conceptuelle afin de pouvoir effectuer les tâches restantes.  
  
![déploiement](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**de la batterie de serveurs fédérés liste de vérification : déploiement d’une batterie de serveurs de Fédération**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Passez en revue les concepts importants et les considérations à prendre en compte lors de la préparation du déploiement de Services ADFS \(AD FS\). **Remarque :**|![déploiement de la batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS Guide de conception dans Windows server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)<p>![déploiement de la batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[comprendre les concepts clés AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)|  
||Si vous décidez d’utiliser Microsoft SQL Server pour votre magasin de configuration AD FS, prenez soin de déployer une instance fonctionnelle de SQL Server.|[SQL Server](https://technet.microsoft.com/sqlserver) **Avertissement :** dans Windows Server 2012 R2, si vous souhaitez créer une batterie de serveurs AD FS et utiliser SQL Server pour stocker vos données de configuration, vous pouvez utiliser SQL Server 2008 et versions plus récentes, y compris SQL Server 2012.|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Joignez votre ordinateur à un domaine Active Directory.|![déploiement de la batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[joindre un ordinateur à un domaine](Join-a-Computer-to-a-Domain.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Inscrire un certificat SSL (Secure Socket Layer) \(SSL\) pour AD FS.|![déploiement de la batterie de serveurs fédérés](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[inscrire un certificat SSL pour AD FS](Enroll-an-SSL-Certificate-for-AD-FS.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Installez le service de rôle AD FS.|![déploiement](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[de la batterie de serveurs fédérés installer le service de rôle AD FS](Install-the-AD-FS-Role-Service.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Configurez un serveur de fédération.|![déploiement](media/bc6cea1a-1c6c-4124-8c8f-1df5adfe8c88.gif)[de la batterie de serveurs fédérés configurer un serveur de Fédération](Configure-a-Federation-Server.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Étape facultative : configurer un serveur de Fédération avec Device Registration service \(DRS\).|![déploiement de la batterie de serveurs fédérés](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configurer un serveur de Fédération avec Device Registration service](Configure-a-federation-server-with-Device-Registration-Service.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Ajoutez un ordinateur hôte \(un\) et un alias \(enregistrement de ressource CNAMe\) à l' \(DNS du système de nom de domaine d’entreprise\) pour le service de Fédération et DRS.|![déploiement](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[de la batterie de serveurs fédérés configurer le système DNS d’entreprise pour les service FS (Federation Service) et Drs](Configure-Corporate-DNS-for-the-Federation-Service-and-DRS.md)|  
|![déploiement de la batterie de serveurs fédérés](media/icon_checkboxo.gif)|Vérifiez qu’un serveur de fédération est opérationnel.|![déploiement](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[de la batterie de serveurs fédérés vérifier qu’un serveur de Fédération est opérationnel](Verify-That-a-Federation-Server-Is-Operational.md)|  
  

## <a name="see-also"></a>Voir aussi  
[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
  

