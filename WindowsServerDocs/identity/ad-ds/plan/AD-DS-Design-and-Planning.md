---
ms.assetid: a91339ef-6ad4-445f-8ecc-a95fbcc04296
title: Planification et conception des services AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5f88458c8e3f50853229f6f8f9c74fa4b8feba40
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624417"
---
# <a name="ad-ds-design-and-planning"></a>Planification et conception des services AD DS

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En déployant des Active Directory Domain Services Windows Server (AD DS) dans votre environnement, vous pouvez tirer parti du modèle d’administration délégué et centralisé et des fonctionnalités d’authentification unique (SSO) fournies par AD DS. Une fois que vous avez identifié les tâches de déploiement et l’environnement actuel de votre organisation, vous pouvez créer la stratégie de déploiement AD DS qui répond aux besoins de votre organisation.

## <a name="about-this-guide"></a>À propos de ce guide

Ce guide fournit des recommandations pour vous aider à développer une stratégie de déploiement AD DS en fonction des besoins de votre organisation et de la conception particulière que vous souhaitez créer. Ce guide est destiné aux spécialistes des infrastructures ou aux architectes système. Avant de lire ce guide, vous devez avoir une bonne compréhension de la façon dont AD DS fonctionne sur un niveau fonctionnel. Vous devez également avoir une bonne compréhension des exigences organisationnelles qui seront reflétées dans votre stratégie de déploiement AD DS.

Ce guide décrit les ensembles de tâches pour plusieurs points de départ possibles d’un déploiement de Windows Server 2008 AD DS. Le guide vous aide à déterminer la stratégie de déploiement la plus appropriée pour votre environnement.

Bien que les stratégies présentées dans ce guide soient appropriées pour presque tous les déploiements de système d’exploitation serveur, elles ont été testées et validées spécifiquement pour les environnements qui contiennent moins de 100 000 utilisateurs et moins de 1 000 sites, avec des connexions réseau d’un minimum de 28,8 kilobits par seconde (Kbits/s). Si votre environnement ne répond pas à ces critères, envisagez d’utiliser une société de Conseil qui a l’expérience de déployer des AD DS dans des environnements plus complexes.

Pour plus d’informations sur le test de la AD DS processus de déploiement, consultez l’article [test et vérification du processus de déploiement](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc772722(v=ws.10)).

## <a name="in-this-guide"></a>Contenu de ce guide

[Présentation de la conception AD DS](Understanding-AD-DS-Design.md)

[Identification des exigences en matière de conception et de déploiement AD DS](Identifying-Your-AD-DS-Design-and-Deployment-Requirements.md)

[Mappage de vos besoins à une stratégie de déploiement AD DS](Mapping-Your-Requirements-to-an-AD-DS-Deployment-Strategy.md)

[Conception de la structure logique pour Windows Server 2008 AD DS](Designing-the-Logical-Structure.md)

[Conception de la topologie de site pour Windows Server 2008 AD DS](Designing-the-Site-Topology.md)

[Activation des fonctionnalités avancées pour AD DS](Enabling-Advanced-Features-for-AD-DS.md)

[Évaluation des exemples de stratégies de déploiement AD DS](Evaluating-AD-DS-Deployment-Strategy-Examples.md)

[Annexe A : Révision des principaux termes relatifs à AD DS](Appendix-A--Reviewing-Key-AD-DS-Terms.md)
