---
ms.assetid: a91339ef-6ad4-445f-8ecc-a95fbcc04296
title: "Planification et conception d’ADDS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 150e79f6ba89208e617ff0c487d64f415aac538e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-design-and-planning"></a>Planification et conception d’ADDS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

En déployant les Services de domaine d’ActiveDirectory Windows Server (ADDS) dans votre environnement, vous pouvez tirer parti du modèle d’administration centralisée, délégué et unique (SSO) fonctionnalité d’authentification qui fournit les services ADDS. Après avoir identifié les tâches de déploiement et l’environnement actuel pour votre organisation, vous pouvez créer la stratégie de déploiement des services ADDS qui répond aux besoins de votre organisation.  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide fournit des recommandations pour vous aider à développer une stratégie de déploiement d’ADDS basée sur les exigences de votre organisation et de la conception que vous souhaitez créer. Ce guide est destiné à être utilisé par les spécialistes d’infrastructure ou aux architectes système. Avant de lire ce guide, vous devez avoir une bonne compréhension du fonctionnement des services ADDS sur un niveau fonctionnel. Vous devez également avoir une bonne compréhension des exigences d’organisation qui sera répercutée dans votre stratégie de déploiement des services ADDS.  
  
Ce guide décrit les ensembles de tâches pour plusieurs points de départ possibles d’un déploiement d’ADDS Windows Server2008. Le guide vous aide à déterminer la stratégie de déploiement la plus appropriée pour votre environnement.  
  
Bien que les stratégies qui sont présentées dans ce guide sont appropriées pour presque tous les déploiements de système d’exploitation de serveur, ils ont été testées et validées spécifiquement pour les environnements qui contiennent moins de 100000utilisateurs et moins de 1000sites, avec un minimum de 28,8kilobits par seconde (Kbits/s), les connexions réseau. Si votre environnement ne répond pas à ces critères, envisagez d’utiliser une société de conseil qui a l’expérience dans le déploiement des services ADDS dans les environnements plus complexes.  
  
Pour plus d’informations sur le processus de déploiement d’ADDS, voir le test et la vérification du processus de déploiement ([https://go.microsoft.com/fwlink/?LinkId=100206](https://go.microsoft.com/fwlink/?LinkId=100206)).  
  
> [!NOTE]  
> Les informations contenues dans ce guide s’applique également à Windows Server2008R2 ADDS.  
  
## <a name="in-this-guide"></a>Dans ce guide  
[Présentation de conception d’ADDS](Understanding-AD-DS-Design.md)  
  
[Identification de votre conception d’ADDS et la configuration requise du déploiement](Identifying-Your-AD-DS-Design-and-Deployment-Requirements.md)  
  
[Mappage de vos besoins pour une stratégie de déploiement d’ADDS](Mapping-Your-Requirements-to-an-AD-DS-Deployment-Strategy.md)  
  
[Conception de la Structure logique pour Windows Server2008, ADDS](Designing-the-Logical-Structure.md)  
  
[Conception de la topologie de Site pour Windows Server2008, ADDS](Designing-the-Site-Topology.md)  
  
[Activation des fonctionnalités avancées pour ADDS](Enabling-Advanced-Features-for-AD-DS.md)  
  
[Évaluation des exemples de stratégies de déploiement ADDS](Evaluating-AD-DS-Deployment-Strategy-Examples.md)  
  
[Annexe a: examen des termes du contrat de clé ADDS](Appendix-A--Reviewing-Key-AD-DS-Terms.md)  
  


