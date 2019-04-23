---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: Conception de la structure logique
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 82184fe678c05b1d7458584de8eecd0c07ece02f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832330"
---
# <a name="designing-the-logical-structure"></a>Conception de la structure logique

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Services de domaine Active Directory (AD DS) permet aux organisations de créer une infrastructure évolutive, sécurisée et gérable pour la gestion des utilisateurs et ressources. Elle leur permet également de prendre en charge des applications d’annuaire.  
  
Une structure logique Active Directory bien conçue offre les avantages suivants :  
  
-   Gestion simplifiée des réseaux Windows Microsoft qui contiennent un grand nombre d’objets  
  
-   Une structure de domaine consolidé et les coûts d’administration réduits  
  
-   La possibilité de déléguer le contrôle administratif sur les ressources, comme il convient  
  
-   Impact réduit sur la bande passante réseau  
  
-   Partage des ressources simplifiée  
  
-   Performances de recherche optimale  
  
-   Faible coût total de possession  
  
Une structure logique Active Directory bien conçue facilite l’intégration efficace des fonctionnalités telles que la stratégie de groupe ; verrouillage des postes ; distribution de logiciels ; et l’administration utilisateur, groupe, station de travail et serveur dans votre système. En outre, une structure logique soigneusement conçue facilite l’intégration de Microsoft et non Microsoft services et applications, telles que Microsoft Exchange Server, infrastructure à clé publique (PKI), et basés sur un domaine fichier système distribués (DFS).  
  
Lorsque vous concevez une structure logique Active Directory avant de déployer les services AD DS, vous pouvez optimiser votre processus de déploiement pour tirer le meilleur parti des fonctionnalités d’Active Directory. Pour concevoir la structure logique Active Directory, votre équipe de conception tout d’abord identifie la configuration requise pour votre organisation et, en fonction de ces informations, décide où placer les limites de la forêt et du domaine. L’équipe de conception décide alors, comment configurer l’environnement du système DNS (Domain Name) pour répondre aux besoins de la forêt. Enfin, l’équipe de conception identifie la structure d’unité d’organisation (UO) qui est nécessaire pour déléguer la gestion des ressources de votre organisation.  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Présentation du modèle logique Active Directory](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [Identifier les participants au projet de déploiement](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [Création d’une conception de forêt](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [Création d’un domaine](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [Création d’une conception d’Infrastructure DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [Création d’une conception d’unité d’organisation](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [Annexe a : Inventaire DNS](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


