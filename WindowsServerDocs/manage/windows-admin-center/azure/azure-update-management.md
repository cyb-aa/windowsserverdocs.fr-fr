---
title: Utilisez Windows Admin Center pour gérer les mises à jour du système d’exploitation avec la gestion des mises à jour Azure
description: Utiliser Windows Admin Center (projet Honolulu) pour configurer la gestion des mises à jour Azure pour gérer le système d’exploitation met à jour.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 5ba81968f8baa81176ad646fb2a97961ddc49fda
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296914"
---
# Utilisez Windows Admin Center pour gérer les mises à jour du système d’exploitation avec la gestion des mises à jour Azure

[En savoir plus sur l’intégration d’Azure avec Windows Admin Center.](../plan/azure-integration-options.md)

Gestion de mise à jour Microsoft Azure est une solution dans Azure Automation qui vous permet de gérer les mises à jour et des correctifs pour plusieurs ordinateurs à partir d’un emplacement unique, plutôt que sur chaque serveur. Avec la gestion des mises à jour Azure, vous pouvez rapidement évaluer l’état des mises à jour disponibles, planifier l’installation des mises à jour requises et passer en revue les résultats de déploiement pour vérifier les mises à jour qui s’appliquent avec succès. Cela est possible si vos ordinateurs sont les machines virtuelles Azure, hébergée par d’autres fournisseurs de cloud, ou sur site. [En savoir plus sur la gestion des mises à jour Azure.](https://docs.microsoft.com/azure/automation/automation-update-management)

Avec Windows Admin Center, vous pouvez facilement configurer et utiliser la gestion de mise à jour d’Azure pour maintenir vos serveurs gérés à jour. Si vous n’avez pas encore un espace de travail Analytique du journal dans votre abonnement Azure, Windows Admin Center sera automatiquement configurer votre serveur et créer des ressources Azure nécessaires dans l’abonnement et l’emplacement que vous spécifiez. Si vous avez un espace de travail Analytique du journal existant, Windows Admin Center peut configurer automatiquement votre serveur pour utiliser les mises à jour à partir de la gestion des mises à jour Azure.  

Pour commencer, accédez à l’outil de mises à jour dans une connexion serveur et sélectionnez «Configurer maintenant» et fournir vos préférences pour les ressources Azure associés. 

Une fois que vous avez configuré votre serveur pour être géré par la gestion des mises à jour Azure, vous pouvez accéder à gestion des mises à jour Azure à l’aide de l’élément hyperlink fournie dans l’outil de mises à jour. 

[Découvrez comment arrêter l’utilisation de la gestion de mettre à jour Azure pour mettre à jour de votre serveur.](azure-monitor.md#disabling-monitoring)

Notez que vous devez [inscrire votre passerelle Windows Admin Center avec Azure](..\configure\azure-integration.md) avant de configurer la gestion des mises à jour Azure.

