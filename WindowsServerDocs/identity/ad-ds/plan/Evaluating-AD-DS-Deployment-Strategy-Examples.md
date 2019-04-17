---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: "Évaluation des exemples de stratégies de déploiement ADDS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4cac1e344cfed078927f2945048807d686deebd1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Évaluation des exemples de stratégies de déploiement ADDS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Prenons l’exemple suivant d’une société fictive, Contoso Pharmaceuticals, qui est de déploiement des Services de domaine ActiveDirectory (ADDS) dans son environnement. L’environnement Contoso se compose de quatre domaines. Le niveau fonctionnel de forêt est Windows Server2003. L’illustration suivante montre la structure de domaine en cours de l’organisation Contoso.  
  
![Stratégie de déploiement ADDS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
Après avoir vérifié qu’il est l’environnement existant et identification des objectifs de son déploiement, Contoso établi la stratégie de déploiement des services ADDS suivante:  
  
-   Mettre à niveau des domaines Windows Server2003 à des domaines Windows Server2008.  
  
-   Activer les fonctionnalités ADDS avancées en lançant les niveaux fonctionnels de domaine et de forêt vers Windows Server2008.  
  
-   Restructuration des domaines dans la forêt de consolidation de ce domaine avec le domaine emea.concorp.contoso.con africa.concorp.contoso.com.  
  
Augmenter le niveau fonctionnel de forêt vers Windows Server2008 permettra à Contoso tirer pleinement parti des nouvelles fonctionnalités ADDS. Restructuration de domaines dans la forêt, comme illustré dans l’illustration suivante, permet de réduire la quantité d’administration est nécessaire pour la gestion des domaines.  
  
![Stratégie de déploiement ADDS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


