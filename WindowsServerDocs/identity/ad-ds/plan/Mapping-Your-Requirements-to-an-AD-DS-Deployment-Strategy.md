---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: Mappage de vos besoins à une stratégie de déploiement des services AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 412e1ccb13b4d92801faf019b1ed6d01eff8b8d4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402532"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Mappage de vos besoins à une stratégie de déploiement des services AD DS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une fois que vous avez examiné et identifié les exigences de conception et de déploiement de Active Directory Domain Services (AD DS) et que vous déterminez celles qui sont liées à votre déploiement spécifique, vous pouvez mapper ces spécifications à un déploiement de AD DS spécifique. stratégie.  
  
Utilisez le tableau suivant pour déterminer quelle AD DS stratégie de déploiement correspond à la combinaison appropriée de AD DS exigences de conception et de déploiement pour votre organisation. (« Oui » signifie qu’une exigence spécifique est nécessaire pour votre stratégie de déploiement ; « Non » signifie qu’une exigence spécifique n’est pas nécessaire pour votre stratégie de déploiement.)  
  
Ce tableau se rapporte uniquement aux trois principales stratégies de déploiement AD DS, comme décrit dans ce guide :  
  
-   [Déploiement d’AD DS dans une nouvelle organisation](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Déploiement d’AD DS dans une organisation Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Déploiement d’AD DS dans une organisation Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
Toutefois, vous pouvez créer une stratégie de déploiement AD DS hybride ou personnalisé à l’aide de n’importe quelle combinaison des AD DS exigences de conception et de déploiement pour répondre aux besoins de votre organisation.  
  
|Exigences relatives à la conception et au déploiement de AD DS|Déploiement des services AD DS dans une nouvelle organisation|Déploiement des services ADDS dans une organisation Windows Server2003|Déploiement des services AD DS dans une organisation Windows 2000|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Conception de la structure logique pour Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc770806.aspx)|Oui|Oui|Oui|  
|[Conception de la topologie de site pour Windows Server 2008 AD DS](Designing-the-Site-Topology.md)|Oui|Oui|Oui|  
|Planification de la capacité des contrôleurs de domaine|Oui|Oui|Oui|  
|[Déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx)|Oui|Non|Non|  
|[Déploiement de domaines régionaux Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx)|Oui|Oui|Oui|  
|[Activation des fonctionnalités avancées pour AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|Oui|Oui, mais tous les contrôleurs de domaine de l’environnement doivent exécuter Windows Server 2008 avant de définir le niveau fonctionnel du domaine ou de la forêt sur Windows Server 2008.|Oui, mais tous les contrôleurs de domaine de l’environnement doivent exécuter Windows Server 2008 avant de définir le niveau fonctionnel du domaine ou de la forêt sur Windows Server 2008.|  
|[Mise à niveau de domaines Active Directory vers des domaines Windows Server 2008 et Windows Server 2008 R2 AD DS](https://technet.microsoft.com/library/cc731188.aspx)|Non|Oui|Oui|  
|[Restructuration de AD DS domaines entre forêts](https://go.microsoft.com/fwlink/?LinkId=93678)|Oui, si vous souhaitez migrer un domaine pilote dans votre environnement de production, effectuer une fusion avec une autre organisation et consolider les deux infrastructures informatiques, ou consolider les domaines de ressources et de comptes que vous avez mis à niveau en place à partir de Windows 2000 ou les environnements Windows Server 2003.|Oui, si vous souhaitez fusionner avec une autre organisation et consolider les deux infrastructures informatiques ou consolider les domaines de ressources et de comptes que vous avez mis à niveau en place à partir des environnements Windows 2000 ou Windows Server 2003.|Oui, si vous souhaitez fusionner avec une autre organisation et consolider les deux infrastructures informatiques ou consolider les domaines de ressources et de comptes que vous avez mis à niveau en place à partir des environnements Windows 2000 ou Windows Server 2003.|  
|[Restructuration de AD DS domaines au sein des forêts](https://go.microsoft.com/fwlink/?LinkId=82740))|Non|Oui, si vous devez réduire le nombre de domaines, réduire le trafic de réplication et la quantité d’administration utilisateur et de groupe requise, ou simplifier l’administration de stratégie de groupe.|Oui, si vous devez réduire le nombre de domaines, réduire le trafic de réplication et la quantité d’administration utilisateur et de groupe requise, ou simplifier l’administration de stratégie de groupe.|  
  


