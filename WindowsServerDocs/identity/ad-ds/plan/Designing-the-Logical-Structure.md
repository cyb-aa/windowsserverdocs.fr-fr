---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: Conception de la structure logique
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: de56205c163abff1b05d57ea90954fa93606abce
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822612"
---
# <a name="designing-the-logical-structure"></a>Conception de la structure logique

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) permet aux organisations de créer une infrastructure évolutive, sécurisée et gérable pour la gestion des utilisateurs et des ressources. Il leur permet également de prendre en charge les applications compatibles avec l’annuaire.  
  
Une structure logique Active Directory bien conçue offre les avantages suivants :  
  
-   Gestion simplifiée des réseaux basés sur Microsoft Windows qui contiennent un grand nombre d’objets  
  
-   Une structure de domaine consolidée et des coûts d’administration réduits  
  
-   La possibilité de déléguer le contrôle administratif sur les ressources, le cas échéant  
  
-   Impact réduit sur la bande passante réseau  
  
-   Partage de ressources simplifié  
  
-   Performances de recherche optimales  
  
-   Faible coût total de possession  
  
Une structure logique Active Directory bien conçue facilite l’intégration efficace de telles fonctionnalités comme stratégie de groupe ; verrouillage des postes de travail distribution de logiciels ; et l’administration des utilisateurs, des groupes, des stations de travail et des serveurs dans votre système. En outre, une structure logique soigneusement conçue facilite l’intégration des applications et services Microsoft et non-Microsoft, tels que Microsoft Exchange Server, l’infrastructure de clé publique (PKI) et un système de fichiers distribués (DFS) basé sur un domaine.  
  
Lorsque vous concevez une structure logique Active Directory avant de déployer AD DS, vous pouvez optimiser votre processus de déploiement pour tirer le meilleur parti des fonctionnalités de Active Directory. Pour concevoir la structure logique Active Directory, votre équipe de conception identifie d’abord les exigences de votre organisation et, en fonction de ces informations, décide où placer les limites de forêt et de domaine. L’équipe de conception décide ensuite comment configurer l’environnement DNS (Domain Name System) pour répondre aux besoins de la forêt. Enfin, l’équipe de conception identifie la structure de l’unité d’organisation (UO) qui est requise pour déléguer la gestion des ressources dans votre organisation.  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Fonctionnement du modèle logique Active Directory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [Identification des participants au projet de déploiement](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [Création d’une conception de forêt](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [Création d’une conception de domaine](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [Création d’une conception d’infrastructure DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [Création d’une conception d’unité d’organisation](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [Annexe A : inventaire DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


