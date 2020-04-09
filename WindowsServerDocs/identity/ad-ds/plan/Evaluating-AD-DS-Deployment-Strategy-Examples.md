---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: Évaluation des exemples de stratégies de déploiement des services AD DS
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 228aec00fab5e1ca4e136a0f75cb5e2294d66700
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822492"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Évaluation des exemples de stratégies de déploiement des services AD DS

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Prenons l’exemple suivant d’une société fictive, contoso Pharmaceuticals, qui déploie Active Directory Domain Services (AD DS) dans son environnement. L’environnement contoso est constitué de quatre domaines. Le niveau fonctionnel de la forêt est Windows Server 2003. L’illustration suivante montre la structure de domaine actuelle pour l’organisation contoso.  
  
![Stratégie de déploiement de AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
Après avoir examiné l’environnement existant et identifié ses objectifs de déploiement, Contoso a établi la stratégie de déploiement AD DS suivante :  
  
-   Mettez à niveau les domaines Windows Server 2003 vers des domaines Windows Server 2008.  
  
-   Activez les fonctionnalités de AD DS avancées en augmentant les niveaux fonctionnels du domaine et de la forêt à Windows Server 2008.  
  
-   Restructurez le domaine africa.concorp.contoso.com au sein de la forêt pour consolider ce domaine avec le domaine EMEA. concorp. contoso. con.  
  
L’augmentation du niveau fonctionnel de la forêt à Windows Server 2008 permettra à contoso de tirer pleinement parti des nouvelles fonctionnalités de AD DS. La restructuration des domaines au sein de la forêt, comme indiqué dans l’illustration suivante, réduira la quantité d’administration nécessaire à la gestion des domaines.  
  
![Stratégie de déploiement de AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


