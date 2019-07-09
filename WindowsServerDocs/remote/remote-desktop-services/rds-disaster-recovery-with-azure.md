---
title: Configurer la reprise d’activité après sinistre pour les services Bureau à distance avec Azure Site Recovery
description: Découvrir comment utiliser Azure Site Recovery pour la reprise d’activité après sinistre dans un déploiement des services Bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 06/12/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 24b5fdaa815b6d2e84606cd8e681634eb3d0f4e9
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63713079"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>Configurer la reprise d’activité après sinistre pour les services Bureau à distance avec Azure Site Recovery

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Vous pouvez utiliser Azure Site Recovery pour créer une solution de reprise d’activité après sinistre de votre déploiement des services Bureau à distance. 

[Azure Site Recovery](/azure/site-recovery/site-recovery-overview) est un service Azure qui assure des fonctionnalités de reprise d’activité après sinistre, en coordonnant la réplication, le basculement et la récupération des machines virtuelles. Azure Site Recovery prend en charge de nombreuses technologies de réplication, afin de systématiquement répliquer, protéger et basculer sans interruption machines virtuelles et applications vers des clouds publics, privés ou proposés par l’hébergeur. 

Utilisez les informations suivantes pour créer et valider la solution de reprise d’activité après sinistre.

## <a name="disaster-recovery-deployment-options"></a>Options de déploiement de la reprise d’activité après sinistre

Vous pouvez déployer les services Bureau à distance sur des serveurs physiques ou des machines virtuelles exécutant Hyper-V ou VMWare. Azure Site Recovery peut protéger des déploiements virtuels et locaux, sur un site secondaire ou sur Azure. Le tableau suivant présente plusieurs déploiements de services Bureau à distance pris en charge dans des scénarios de reprise d’activité après sinistre, de site à site, et de site à Azure.

| Type de déploiement                          | Site Hyper-V à site | Site Hyper-V à Azure | Site VMWare à Azure | Site physique à Azure |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| Bureau virtuel mis en pool (non managé)       |Oui|Non|Non|Non |
| Bureau virtuel mis en pool (managé, sans UPD) | Oui|Non|Non|Non|
| Sessions RemoteApps et de bureau (sans UPD) | Oui|Oui|Oui|Oui  |

## <a name="prerequisites"></a>Conditions préalables

Avant de pouvoir configurer Azure Site Recovery pour votre déploiement, assurez-vous que les conditions suivantes sont remplies :

- Créez un [déploiement des services Bureau à distance en local](rds-deploy-infrastructure.md).
- Ajoutez un [coffre des services Azure Site Recovery](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault) à votre abonnement Microsoft Azure.
- Si vous vous apprêtez à utiliser Azure en tant que site de reprise d’activité, exécutez l’[outil d’évaluation de la préparation des machines virtuelles Azure](https://azure.microsoft.com/downloads/vm-readiness-assessment/) sur vos machines virtuelles pour vérifier qu’elles sont compatibles avec les machines virtuelles Azure et les services Azure Site Recovery.
 
## <a name="implementation-checklist"></a>Check-list d’implémentation

Nous allons aborder plus en détail les différentes étapes de l’activation des services Azure Site Recovery pour votre déploiement des services Bureau à distance, mais dans l’immédiat attachons-nous aux étapes générales de l’implémentation.

| **Étape 1 - Configuration des machines virtuelles pour la reprise d’activité après sinistre**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-V : téléchargez le fournisseur Microsoft Azure Site Recovery. Installez-le sur votre serveur VMM ou votre hôte Hyper-V. Consultez [Prérequis de la réplication vers Azure avec Azure Site Recovery](/azure/site-recovery/site-recovery-prereq) pour plus d’informations.                                                                                                                             |
| VMWare : configurez le serveur de protection, le serveur de configuration et les serveurs cibles maîtres.                                                                                                                                                      |
| **Étape 2 - Préparation de vos ressources**                                                                                                                                                                                                           |
| Ajoutez un [compte de stockage Azure](/azure/storage/storage-create-storage-account).                                                                                                                                                                                                              |
| Hyper-V : téléchargez l’agent Microsoft Azure Recovery Services et installez-le sur les serveurs hôtes Hyper-V.                                                                                                                                     |
| VMWare : vérifiez que le service Mobilité est installé sur toutes les machines virtuelles.                                                                                                                                                                           |
| [Activez la protection des machines virtuelles dans le cloud VMM, les sites Hyper-V ou les sites VMWare](rds-enable-dr-with-asr.md).                                                                                                                                                                    |
| **Étape 3 - Conception de votre plan de reprise**                                                                                                                                                                                                        |
| Mappage de vos ressources : mappez les réseaux locaux aux réseaux virtuels Azure.                                                                                                                                                                              |
| [Créez le plan de reprise](rds-disaster-recovery-plan.md). |
| Testez le plan de reprise en créant un test de basculement. Vérifiez que toutes les machines virtuelles peuvent accéder aux ressources nécessaires, comme Active Directory. Assurez-vous que les redirections de réseau sont configurées et qu’elles fonctionnent pour les services Bureau à distance. Pour obtenir des instructions détaillées sur le test de votre plan de reprise, consultez [Effectuer un test de basculement](/azure/site-recovery/site-recovery-test-failover-to-azure)|
| **Étape 4 - Exécution d’une extraction de reprise d’activité**                                                                                                                                                                                                     |
| Effectuez une extraction de reprise d’activité à l’aide des basculements planifiés et non planifiés. Assurez-vous que toutes les machines virtuelles ont accès aux ressources nécessaires, telles qu’Active Directory. Assurez-vous que toutes les machines virtuelles ont accès aux ressources nécessaires, telles qu’Active Directory. Pour obtenir des instructions détaillées sur les basculements et la façon de procéder aux extractions, consultez [Basculement dans Site Recovery](/azure/site-recovery/site-recovery-failover).|


