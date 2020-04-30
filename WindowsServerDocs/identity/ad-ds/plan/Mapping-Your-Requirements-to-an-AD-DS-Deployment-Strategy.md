---
ms.assetid: ce3be131-06ad-41dc-a26b-1168fa68c8ed
title: Mappage de vos besoins à une stratégie de déploiement des services AD DS
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4862116253f51b7072f112c5f6ece72605b63ddb
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624097"
---
# <a name="mapping-your-requirements-to-an-ad-ds-deployment-strategy"></a>Mappage de vos besoins à une stratégie de déploiement des services AD DS

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une fois que vous avez examiné et identifié les exigences de conception et de déploiement de Active Directory Domain Services (AD DS) et que vous déterminez celles qui sont liées à votre déploiement spécifique, vous pouvez mapper ces exigences à une stratégie de déploiement de AD DS spécifique.

Utilisez le tableau suivant pour déterminer quelle AD DS stratégie de déploiement correspond à la combinaison appropriée de AD DS exigences de conception et de déploiement pour votre organisation. (« Oui » signifie qu’une exigence spécifique est nécessaire pour votre stratégie de déploiement ; « Non » signifie qu’une exigence spécifique n’est pas nécessaire pour votre stratégie de déploiement.)

Ce tableau se rapporte uniquement aux trois principales stratégies de déploiement AD DS, comme décrit dans ce guide :

-   [Déploiement d’AD DS dans une nouvelle organisation](../../ad-ds/plan/Deploying-AD-DS-in-a-New-Organization.md)

-   [Déploiement d’AD DS dans une organisation Windows Server 2003](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-Server-2003-Organization.md)

-   [Déploiement d’AD DS dans une organisation Windows 2000](../../ad-ds/plan/Deploying-AD-DS-in-a-Windows-2000-Organization.md)

Toutefois, vous pouvez créer une stratégie de déploiement AD DS hybride ou personnalisé à l’aide de n’importe quelle combinaison des AD DS exigences de conception et de déploiement pour répondre aux besoins de votre organisation.

| Exigences relatives à la conception et au déploiement de AD DS | Déploiement des services AD DS dans une nouvelle organisation | Déploiement des services AD DS dans une organisation Windows Server 2003 | Déploiement des services AD DS dans une organisation Windows 2000 |
| ---------------------------------------- | ------------------------------------- | ----------------------------------------------------- |----------------------------------------------- |
| [Conception de la structure logique pour Windows Server 2008 AD DS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770806(v=ws.10)) | Oui | Oui | Oui |
| [Conception de la topologie de site pour Windows Server 2008 AD DS](Designing-the-Site-Topology.md) | Oui | Oui | Oui |
| Planification de la capacité des contrôleurs de domaine | Oui | Oui | Oui |
| [Déploiement d’un domaine racine de forêt Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731174(v=ws.10)) | Oui | Non  | Non  |
| [Déploiement de domaines régionaux Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755118(v=ws.10)) | Oui | Oui | Oui |
| [Activation des fonctionnalités avancées pour AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md) | Oui |Oui, mais tous les contrôleurs de domaine de l’environnement doivent exécuter Windows Server 2008 avant de définir le niveau fonctionnel du domaine ou de la forêt sur Windows Server 2008. | Oui, mais tous les contrôleurs de domaine de l’environnement doivent exécuter Windows Server 2008 avant de définir le niveau fonctionnel du domaine ou de la forêt sur Windows Server 2008. |
| [Mise à niveau de domaines Active Directory vers des domaines Windows Server 2008 et Windows Server 2008 R2 AD DS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731188(v=ws.10)) | Non | Oui | Oui |
| [Guide de l’outil ADMT : migration et restructuration de Active Directory domaines](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc974332(v=ws.10)) | Oui, si vous souhaitez migrer un domaine pilote dans votre environnement de production, effectuer une fusion avec une autre organisation et consolider les deux infrastructures informatiques, ou consolider les domaines de ressources et de comptes que vous avez mis à niveau en place à partir des environnements Windows 2000 ou Windows Server 2003. | Oui, si vous souhaitez fusionner avec une autre organisation et consolider les deux infrastructures informatiques ou consolider les domaines de ressources et de comptes que vous avez mis à niveau en place à partir des environnements Windows 2000 ou Windows Server 2003. | Oui, si vous souhaitez fusionner avec une autre organisation et consolider les deux infrastructures informatiques ou consolider les domaines de ressources et de comptes que vous avez mis à niveau en place à partir des environnements Windows 2000 ou Windows Server 2003. |
| [Guide de l’outil ADMT : migration et restructuration de Active Directory domaines](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc974332(v=ws.10)) | Non  | Oui, si vous devez réduire le nombre de domaines, réduire le trafic de réplication et la quantité d’administration utilisateur et de groupe requise, ou simplifier l’administration de stratégie de groupe. | Oui, si vous devez réduire le nombre de domaines, réduire le trafic de réplication et la quantité d’administration utilisateur et de groupe requise, ou simplifier l’administration de stratégie de groupe. |
