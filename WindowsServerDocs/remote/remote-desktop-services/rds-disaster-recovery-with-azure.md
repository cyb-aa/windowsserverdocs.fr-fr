---
title: Configurer la récupération d’urgence pour les services Bureau à distance à l’aide de la récupération d’urgence Azure
description: Découvrez comment utiliser la récupération d’urgence Azure pour la récupération d’urgence pour un déploiement services Bureau à distance
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
ms.openlocfilehash: 561a515e23d12cc3397c40fd885550e735ed4d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878170"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>Configurer la récupération d’urgence pour les services Bureau à distance à l’aide d’Azure Site Recovery

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser Azure Site Recovery pour créer une solution de récupération d’urgence pour votre déploiement des Services Bureau à distance. 

[Azure Site Recovery](/azure/site-recovery/site-recovery-overview) est un service basé sur Azure qui fournit des fonctionnalités de récupération après sinistre en coordonnant la réplication, le basculement et récupération des machines virtuelles. Azure Site Recovery prend en charge un nombre de technologies de réplication pour systématiquement répliquer, protéger et en toute transparence basculer des machines virtuelles et les applications à des clouds de l’hébergeur ou publique/privée. 

Utilisez les informations suivantes pour créer et valider la solution de récupération d’urgence.

## <a name="disaster-recovery-deployment-options"></a>Options de déploiement de récupération d’urgence

Vous pouvez déployer des services Bureau à distance sur des serveurs physiques ou machines virtuelles exécutant Hyper-V ou VMWare. Azure Site Recovery peut protéger à la fois en local et des déploiements virtuels sur un site secondaire ou sur Azure. Le tableau suivant présente que les différents déploiements de services Bureau à distance pris en charge dans les scénarios de recvoery d’urgence de site à site et le site vers Azure.

| Type de déploiement                          | Hyper-V à un site | Site Hyper-V vers Azure | Site VMWare vers Azure | Physique de site vers Azure |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| Bureau virtuel mis en pool (non managé)       |Oui|Non|Non|Non |
| Bureau virtuel mis en pool (géré, sans UPD) | Oui|Non|Non|Non|
| Sessions RemoteApps et de bureau (sans UPD) | Oui|Oui|Oui|Oui  |

## <a name="prerequisites"></a>Prérequis

Avant de pouvoir configurer Azure Site Recovery pour votre déploiement, assurez-vous que les conditions suivantes :

- Créer un [déploiement des services Bureau à distance local](rds-deploy-infrastructure.md).
- Ajouter [coffre Azure Site Recovery Services](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault) à votre abonnement Microsoft Azure.
- Si vous vous apprêtez à utiliser Azure comme site de récupération, exécutez le [outil Azure Virtual Machine Readiness Assessment](https://azure.microsoft.com/downloads/vm-readiness-assessment/) sur vos machines virtuelles pour vérifier qu’ils sont compatibles avec les machines virtuelles Azure et Azure Site Recovery Services.
 
## <a name="implementation-checklist"></a>Liste de vérification de mise en œuvre

Nous aborderons les différentes étapes pour activer Azure Site Recovery Services pour votre déploiement des services Bureau à distance plus en détail, mais voici les étapes d’implémentation de haut niveau.

| **Étape 1 : configurer des machines virtuelles pour la récupération d’urgence**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-V - téléchargement du fournisseur Microsoft Azure Site Recovery. Installez-le sur votre serveur VMM ou un hôte Hyper-V. Consultez [conditions préalables pour la réplication vers Azure à l’aide d’Azure Site Recovery](/azure/site-recovery/site-recovery-prereq) pour plus d’informations.                                                                                                                             |
| VMWare - configurer le serveur de protection, serveur de configuration et serveurs cibles maîtres                                                                                                                                                      |
| **Étape 2 : préparer vos ressources**                                                                                                                                                                                                           |
| Ajouter un [compte de stockage Azure](/azure/storage/storage-create-storage-account).                                                                                                                                                                                                              |
| Hyper-V - Téléchargez l’agent Microsoft Azure Recovery Services et l’installer sur les serveurs hôtes Hyper-V.                                                                                                                                     |
| VMWare : Vérifiez que le service mobilité est installé sur toutes les machines virtuelles.                                                                                                                                                                           |
| [Activer la protection des machines virtuelles dans le cloud VMM, les sites Hyper-V ou les sites VMWare](rds-enable-dr-with-asr.md).                                                                                                                                                                    |
| **Étape 3 : concevoir votre plan de récupération.**                                                                                                                                                                                                        |
| Mappez vos ressources - mappage des réseaux locaux aux réseaux virtuels Azure.                                                                                                                                                                              |
| [Créer le plan de récupération](rds-disaster-recovery-plan.md). |
| Tester le plan de récupération en créant un test de basculement. Vérifiez que toutes les machines virtuelles peuvent accéder aux ressources requises, comme Active Directory. Vérifiez les redirections sont configurées de réseau et l’utilisation de RDS. Pour obtenir des instructions détaillées sur le test de votre plan de récupération, consultez [exécuter un test de basculement](/azure/site-recovery/site-recovery-test-failover-to-azure)|
| **Étape 4 : exécuter une simulation de récupération d’urgence.**                                                                                                                                                                                                     |
| Exécuter une simulation de récupération d’urgence à l’aide des basculements planifiés et non planifiés. Assurez-vous que toutes les machines virtuelles ont accès aux ressources requises, telles qu’Active Directory. Assurez-vous que toutes les machines virtuelles ont accès aux ressources requises, telles qu’Active Directory. Pour obtenir des instructions détaillées sur les basculements et comment effectuer des exercices, consultez [basculement dans Site Recovery](/azure/site-recovery/site-recovery-failover).|


