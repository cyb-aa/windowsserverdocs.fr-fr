---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: Évaluation des exemples de stratégies de déploiement des services AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 169d0a55f9fb167390c13ac1c89f8d68427f318d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842110"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Évaluation des exemples de stratégies de déploiement des services AD DS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Prenons l’exemple suivant d’une entreprise fictive, Contoso Pharmaceuticals, qui est de déployer des Services de domaine Active Directory (AD DS) dans son environnement. L’environnement Contoso se compose de quatre domaines. Le niveau fonctionnel de forêt est Windows Server 2003. L’illustration suivante montre la structure actuelle de l’organisation Contoso.  
  
![Stratégie de déploiement AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
Après avoir examiné son environnement existant et identifier ses objectifs de déploiement, Contoso établi la stratégie de déploiement d’AD DS suivante :  
  
-   Mettre à niveau des domaines Windows Server 2003 à des domaines Windows Server 2008.  
  
-   Activer les fonctionnalités avancées de AD DS en déclenchant les niveaux fonctionnels de domaine et de forêt vers Windows Server 2008.  
  
-   Restructurer le domaine de africa.concorp.contoso.com au sein de la forêt à consolider ce domaine avec le domaine emea.concorp.contoso.con.  
  
Augmentation du niveau fonctionnel de forêt vers Windows Server 2008 permettra à Contoso tirer pleinement parti des nouvelles fonctionnalités AD DS. Restructuration de domaines de la forêt, comme indiqué dans l’illustration suivante, réduit la quantité d’administration qui est nécessaire pour la gestion des domaines.  
  
![Stratégie de déploiement AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


