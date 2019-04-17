---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: Conception de la Structure logique
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d1b9c27f05faef49f7fd4228f4ebe689b75d30f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-logical-structure"></a>Conception de la Structure logique

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Services de domaine ActiveDirectory (ADDS) permet aux organisations de créer une infrastructure évolutive, sécurisée et gérable pour l’utilisateur et de gestion des ressources. Il permet également à prendre en charge les applications d’annuaire.  
  
Une structure logique ActiveDirectory bien conçue offre les avantages suivants:  
  
-   Gestion simplifiée des réseaux MicrosoftWindows qui contiennent le grand nombre d’objets  
  
-   Une structure de domaine consolidée et les coûts d’administration réduits  
  
-   La possibilité de déléguer le contrôle administratif sur les ressources appropriées  
  
-   Atténuer l’impact sur la bande passante réseau  
  
-   Partage des ressources simplifiée  
  
-   Performances optimales de recherche  
  
-   Faible coût total de possession  
  
Une structure logique ActiveDirectory bien conçue facilite l’intégration efficace des fonctionnalités telles que la stratégie de groupe; verrouillage de bureau; distribution de logiciels; et l’utilisateur, groupe, station de travail et le serveur administration dans votre système. En outre, une structure logique conçue avec soin facilite l’intégration de Microsoft et non-Microsoft applications et services, tels que MicrosoftExchange Server, l’infrastructure à clé publique (PKI), et basé sur un domaine distribués (DFS) système de fichiers.  
  
Lorsque vous concevez une structure logique ActiveDirectory avant de déployer les services ADDS, vous pouvez optimiser votre processus de déploiement pour tirer le meilleur parti des fonctionnalités d’ActiveDirectory. Pour concevoir la structure logique ActiveDirectory, votre équipe de conception tout d’abord identifie la configuration requise pour votre organisation et, en fonction de ces informations, décide où placer les limites de la forêt et du domaine. Ensuite, l’équipe de conception pour décide comment configurer l’environnement du système DNS (Domain Name) pour répondre aux besoins de la forêt. Enfin, l’équipe de conception identifie la structure d’unité d’organisation (UO) qui est nécessaire pour déléguer la gestion des ressources de votre organisation.  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Présentation du modèle logique ActiveDirectory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [Identifier les participants au projet de déploiement](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [Création d’une conception de forêt](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [Création d’un domaine](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [Création d’une conception d’Infrastructure DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [Création d’une conception d’unité d’organisation](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [Annexe a: DNS inventaire](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


