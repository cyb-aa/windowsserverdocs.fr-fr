---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: "Mappage de vos besoins pour une stratégie de déploiement d’ADDS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b4f625f06fede5b9dc751282cda19b68d7f9e535
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Mappage de vos besoins pour une stratégie de déploiement d’ADDS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Une fois que vous avez terminé de consulter et d’identification de la conception des Services de domaine Active Directory (AD DS) et configuration requise du déploiement et vous Déterminez lequel d'entre eux sont liés à votre déploiement spécifique, vous pouvez mapper ces exigences pour une stratégie de déploiement de domaine Active Directory spécifique.  
  
Utilisez le tableau suivant pour déterminer quelle stratégie de déploiement des services AD DS correspond à la combinaison appropriée de la conception de domaine Active Directory et de déploiement, configuration requise pour votre organisation. («Oui» signifie qu’une condition spécifique est nécessaire pour votre stratégie de déploiement; «Aucun» signifie qu’une condition spécifique n’est pas nécessaire pour votre stratégie de déploiement.)  
  
Ce tableau fait uniquement référence aux trois stratégies de déploiement principales de domaine Active Directory comme décrit dans ce guide:  
  
-   [Déploiement des services ADDS dans une nouvelle organisation](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)  
  
-   [Déploiement des services AD DS dans un Windows Server2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)  
  
-   [Déploiement des services ADDS dans une organisation Windows2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)  
  
Toutefois, vous pouvez créer un hybride ou un personnalisé stratégie de déploiement des services AD DS à l’aide de n’importe quelle combinaison des exigences de conception et de déploiement d’AD DS pour répondre aux besoins de votre organisation.  
  
|Exigences de conception et de déploiement du service d’annuaire Active Directory|Déploiement des services ADDS dans une nouvelle organisation|Déploiement des services AD DS dans un Windows Server2003|Déploiement des services ADDS dans une organisation Windows2000|  
|--------------------------------------------|-----------------------------------------|---------------------------------------------------------|--------------------------------------------------|  
|[Conception de la Structure logique pour Windows Server2008, ADDS](https://technet.microsoft.com/library/cc770806.aspx)|Oui|Oui|Oui|  
|[Conception de la topologie de Site pour Windows Server2008, ADDS](Designing-the-Site-Topology.md)|Oui|Oui|Oui|  
|Planification de la capacité des contrôleurs de domaine|Oui|Oui|Oui|  
|[Déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx)|Oui|N°|N°|  
|[Déploiement de domaines régionaux de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx)|Oui|Oui|Oui|  
|[Activation des fonctionnalités avancées pour ADDS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md)|Oui|Oui, mais tous les contrôleurs de domaine dans l’environnement doivent exécuter Windows Server 2008 avant de définir le niveau fonctionnel du domaine ou de la forêt vers Windows Server 2008.|Oui, mais tous les contrôleurs de domaine dans l’environnement doivent exécuter Windows Server 2008 avant de définir le niveau fonctionnel du domaine ou de la forêt vers Windows Server 2008.|  
|[La mise à niveau de domaines Active Directory pour Windows Server 2008 et Windows Server 2008 R2 domaines AD DS](https://technet.microsoft.com/library/cc731188.aspx)|N°|Oui|Oui|  
|[Restructuration de domaines AD DS entre forêts](https://go.microsoft.com/fwlink/?LinkId=93678)|Oui, si vous souhaitez migrer un domaine pilot dans votre environnement de production, fusionner avec une autre organisation et consolider les deux infrastructures informatiques (IT) ou consolider les domaines de compte de ressource et de mise à niveau en place à partir de l’environnement Windows 2000 ou Windows Server 2003.|Oui, si vous souhaitez fusionner avec une autre organisation et consolider les deux infrastructures informatiques ou consolider les domaines de compte de ressource et de mise à niveau en place à partir de l’environnement Windows 2000 ou Windows Server 2003.|Oui, si vous souhaitez fusionner avec une autre organisation et consolider les deux infrastructures informatiques ou consolider les domaines de compte de ressource et de mise à niveau en place à partir de l’environnement Windows 2000 ou Windows Server 2003.|  
|[Restructuration de domaines AD DS dans les forêts](https://go.microsoft.com/fwlink/?LinkId=82740))|N°|Oui, si vous avez besoin réduire le nombre de domaines, réduire le trafic de réplication et la quantité d’utilisateur requis et d’administration de groupe, ou de simplifier l’administration de stratégie de groupe.|Oui, si vous avez besoin réduire le nombre de domaines, réduire le trafic de réplication et la quantité d’utilisateur requis et d’administration de groupe, ou de simplifier l’administration de stratégie de groupe.|  
  


