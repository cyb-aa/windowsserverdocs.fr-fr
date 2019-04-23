---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: Mappage de vos besoins à une stratégie de déploiement des services AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a61e5e6d429acd92e48a353bc829cb620dd66054
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881830"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Mappage de vos besoins à une stratégie de déploiement des services AD DS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une fois que vous avez terminé l’examen et l’identification de la conception de Services de domaine Active Directory (AD DS) et les exigences de déploiement et vous Déterminez lequel d'entre eux sont liés à votre déploiement spécifique, vous pouvez mapper ces exigences avec un déploiement AD DS spécifique stratégie.  
  
Utilisez le tableau suivant pour déterminer quelle stratégie de déploiement d’AD DS mappe à la combinaison appropriée de la conception des services AD DS et le déploiement, configuration requise pour votre organisation. (« Oui » signifie qu’une condition spécifique est nécessaire pour votre stratégie de déploiement ; « Non » signifie qu’une exigence spécifique n’est pas nécessaire pour votre stratégie de déploiement.)  
  
Ce tableau fait uniquement référence aux trois stratégies de déploiement AD DS principales comme décrit dans ce guide :  
  
-   [Déploiement des services AD DS dans une nouvelle organisation](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Déploiement des services AD DS dans une organisation Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Déploiement des services AD DS dans une organisation de 2000 Windows](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
Toutefois, vous pouvez créer un hybride ou la stratégie de déploiement personnalisée AD DS à l’aide de n’importe quelle combinaison des exigences de conception et de déploiement d’AD DS pour répondre aux besoins de votre organisation.  
  
|Exigences de conception et de déploiement AD DS|Déploiement des services AD DS dans une nouvelle organisation|Déploiement des services ADDS dans une organisation Windows Server2003|Déploiement des services AD DS dans une organisation Windows 2000|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Conception de la Structure logique pour Windows Server 2008, AD DS](https://technet.microsoft.com/library/cc770806.aspx)|Oui|Oui|Oui|  
|[Conception de la topologie de Site pour Windows Server 2008, AD DS](Designing-the-Site-Topology.md)|Oui|Oui|Oui|  
|Planification de la capacité des contrôleurs de domaine|Oui|Oui|Oui|  
|[Déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx)|Oui|Non|Non|  
|[Déploiement de domaines régionaux de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx)|Oui|Oui|Oui|  
|[Activation des fonctionnalités avancées pour AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|Oui|Oui, mais tous les contrôleurs de domaine dans l’environnement doivent exécuter Windows Server 2008 avant de définir le niveau fonctionnel du domaine ou forêt vers Windows Server 2008.|Oui, mais tous les contrôleurs de domaine dans l’environnement doivent exécuter Windows Server 2008 avant de définir le niveau fonctionnel du domaine ou forêt vers Windows Server 2008.|  
|[La mise à niveau de domaines Active Directory pour Windows Server 2008 et Windows Server 2008 R2 domaines AD DS](https://technet.microsoft.com/library/cc731188.aspx)|Non|Oui|Oui|  
|[Restructuration de domaines AD DS entre forêts](https://go.microsoft.com/fwlink/?LinkId=93678)|Oui, si vous souhaitez migrer un domaine de pilote dans votre environnement de production, fusionner avec une autre organisation et consolider les deux infrastructures informatiques (IT) ou consolider les domaines de ressources et compte mis à niveau sur place à partir de Windows 2000 ou les environnements Windows Server 2003.|Oui, si vous souhaitez fusionner avec une autre organisation et consolider les deux infrastructures informatiques ou consolider les domaines de ressources et compte mis à niveau sur place à partir d’environnements Windows 2000 ou Windows Server 2003.|Oui, si vous souhaitez fusionner avec une autre organisation et consolider les deux infrastructures informatiques ou consolider les domaines de ressources et compte mis à niveau sur place à partir d’environnements Windows 2000 ou Windows Server 2003.|  
|[Restructuration de domaines AD DS dans les forêts](https://go.microsoft.com/fwlink/?LinkId=82740))|Non|Oui, si vous avez besoin réduire le nombre de domaines, réduire le trafic de réplication et la quantité d’utilisateurs requis et l’administration de groupe, ou de simplifier l’administration de stratégie de groupe.|Oui, si vous avez besoin réduire le nombre de domaines, réduire le trafic de réplication et la quantité d’utilisateurs requis et l’administration de groupe, ou de simplifier l’administration de stratégie de groupe.|  
  


