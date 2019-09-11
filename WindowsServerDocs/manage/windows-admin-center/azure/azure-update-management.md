---
title: Utilisez le centre d’administration Windows pour gérer les mises à jour du système d’exploitation avec Azure Update Management
description: Utilisez le centre d’administration Windows (Project Honolulu) pour configurer Azure Update Management pour gérer les mises à jour du système d’exploitation.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: ff67355697051a6c36a5143de96a6aec44bf35ca
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865416"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>Utilisez le centre d’administration Windows pour gérer les mises à jour du système d’exploitation avec Azure Update Management

[En savoir plus sur l’intégration d’Azure avec le centre d’administration Windows.](../plan/azure-integration-options.md)

Azure Update Management est une solution dans Azure Automation qui vous permet de gérer des mises à jour et des correctifs pour plusieurs ordinateurs à partir d’un seul emplacement, plutôt que sur une base par serveur. Avec Azure Update Management, vous pouvez rapidement évaluer le statut des mises à jour disponibles, planifier l'installation des mises à jour requises et examiner les résultats des déploiements pour vérifier que les mises à jour ont bien été installées. Cela est possible, que vos ordinateurs soient des machines virtuelles Azure, hébergés par d’autres fournisseurs de Cloud ou en local. [En savoir plus sur Azure Update Management.](https://docs.microsoft.com/azure/automation/automation-update-management)

Avec le centre d’administration Windows, vous pouvez facilement configurer et utiliser Azure Update Management pour maintenir à jour vos serveurs gérés. Si vous ne disposez pas déjà d’un espace de travail Log Analytics dans votre abonnement Azure, le centre d’administration Windows configure automatiquement votre serveur et crée les ressources Azure nécessaires dans l’abonnement et l’emplacement que vous spécifiez. Si vous disposez déjà d’un espace de travail Log Analytics, le centre d’administration Windows peut configurer automatiquement votre serveur pour qu’il consomme des mises à jour à partir d’Azure Update Management.  

Pour commencer, accédez à l’outil mises à jour dans une connexion au serveur et sélectionnez « Configurer maintenant », puis fournissez vos préférences pour les ressources Azure associées. 

Une fois que vous avez configuré votre serveur pour qu’il soit géré par Azure Update Management, vous pouvez accéder à Azure Update Management à l’aide du lien hypertexte fourni dans l’outil mises à jour. 

[Découvrez comment cesser d’utiliser Azure Update Management pour mettre à jour votre serveur.](azure-monitor.md#disabling-monitoring)

Notez que vous devez [inscrire votre passerelle du centre d’administration Windows auprès d’Azure avant de](../configure/azure-integration.md) configurer Azure Update Management.

