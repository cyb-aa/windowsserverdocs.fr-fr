---
title: Utilisez Windows Admin Center pour gérer les mises à jour du système d’exploitation avec la gestion des mises à jour Azure
description: Utilisez Windows Admin Center (projet Honolulu) pour configurer la gestion de mise à jour d’Azure pour gérer le système d’exploitation met à jour.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 79b18e9963fba0993a7f34b1409edba6abfd48f0
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452541"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>Utilisez Windows Admin Center pour gérer les mises à jour du système d’exploitation avec la gestion des mises à jour Azure

[En savoir plus sur l’intégration d’Azure avec Windows Admin Center.](../plan/azure-integration-options.md)

Gestion de mise à jour Azure est une solution dans Azure Automation qui vous permet de gérer les mises à jour et correctifs pour plusieurs ordinateurs à partir d’un emplacement unique, plutôt que sur chaque serveur. Avec la gestion de mise à jour d’Azure, vous pouvez rapidement évaluer l’état des mises à jour disponibles, planifier l’installation des mises à jour requises et passer en revue les résultats de déploiement pour vérifier les mises à jour sont appliquées correctement. Cela est possible si vos machines sont des machines virtuelles Azure, hébergé par d’autres fournisseurs cloud ou en local. [En savoir plus sur la gestion de mise à jour d’Azure.](https://docs.microsoft.com/azure/automation/automation-update-management)

Avec Windows Admin Center, vous pouvez facilement configurer et utiliser la gestion de mise à jour de Azure pour conserver la mise à jour de vos serveurs gérés. Si vous ne disposez pas d’un espace de travail Analytique de journal dans votre abonnement Azure, Windows Admin Center sera automatiquement configurer votre serveur et créer les ressources Azure nécessaires dans l’abonnement et l’emplacement que vous spécifiez. Si vous avez un espace de travail Analytique de journal, Windows Admin Center peut configurer automatiquement votre serveur pour utiliser des mises à jour de la gestion de mise à jour d’Azure.  

Pour commencer, accéder à l’outil de mises à jour dans une connexion serveur et sélectionnez « Configurer maintenant » et indiquez vos préférences pour les ressources Azure connexes. 

Une fois que vous avez configuré votre serveur pour être gérées par la gestion de mise à jour d’Azure, vous pouvez accéder à Azure Update Management à l’aide du lien hypertexte fourni dans l’outil de mises à jour. 

[Apprenez comment arrêter à l’aide de la gestion de mise à jour d’Azure pour mettre à jour de votre serveur.](azure-monitor.md#disabling-monitoring)

Notez que vous devez [inscrire votre passerelle Windows Admin Center auprès d’Azure](../configure/azure-integration.md) avant de configurer la gestion des mises à jour de Azure.

