---
title: Référence des commandes de sauvegarde Windows Server
description: Rubrique relative aux commandes Windows pour la référence des commandes de sauvegarde.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 03de0a65-21f0-4dd7-a3ae-251c98bbf6eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca6a2e17c68e362d8b3d26f67ab3b7e4c7bf86d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829203"
---
# <a name="windows-server-backup-command-reference"></a>Référence des commandes de sauvegarde Windows Server



Les sous-commandes suivantes pour **Wbadmin** fournissent des fonctionnalités de sauvegarde et de récupération à partir d’une invite de commandes.

Pour configurer une planification de sauvegarde, vous devez être membre du groupe **administrateurs** . Pour effectuer toutes les autres tâches à l’aide de cette commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorisation appropriée doit vous avoir été déléguée.

Vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

|Sous-commande|Description|
|----------|-----------|
|[Wbadmin enable backup](wbadmin-enable-backup.md)|Configure et active une planification de sauvegarde quotidienne.|
|[Wbadmin disable backup](wbadmin-disable-backup.md)|Désactive vos sauvegardes quotidiennes.|
|[Wbadmin start backup](wbadmin-start-backup.md)|Exécute une sauvegarde ponctuelle. En cas d’utilisation sans paramètre, utilise les paramètres de la planification de sauvegarde quotidienne.|
|[Wbadmin stop job](wbadmin-stop-job.md)|Arrête l’opération de sauvegarde ou de récupération en cours d’exécution.|
|[Wbadmin get versions](wbadmin-get-versions.md)|Répertorie les détails des sauvegardes récupérables à partir de l’ordinateur local ou, si un autre emplacement est spécifié, à partir d’un autre ordinateur.|
|[Wbadmin get items](wbadmin-get-items.md)|Répertorie les éléments inclus dans une sauvegarde spécifique.|
|[Wbadmin start recovery](wbadmin-start-recovery.md)|Exécute une récupération des volumes, des applications, des fichiers ou des dossiers spécifiés.|
|[Wbadmin get status](wbadmin-get-status.md)|Affiche l’état de l’opération de sauvegarde ou de récupération en cours d’exécution.|
|[Wbadmin get disks](wbadmin-get-disks.md)|Répertorie les disques actuellement en ligne.|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Exécute une récupération de l’état du système.|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|Exécute une sauvegarde de l’état du système.|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Supprime une ou plusieurs sauvegardes de l’état du système.|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|Exécute une récupération du système complet (au moins tous les volumes qui contiennent l’état du système d’exploitation). Cette sous-commande n’est disponible que si vous utilisez l’environnement de récupération Windows.|
|[Wbadmin restore catalog](wbadmin-restore-catalog.md)|Récupère un catalogue de sauvegarde à partir d’un emplacement de stockage spécifié dans le cas où le catalogue de sauvegarde de l’ordinateur local a été endommagé.|
|[Wbadmin delete catalog](wbadmin-delete-catalog.md)|Supprime le catalogue de sauvegarde sur l’ordinateur local. Utilisez cette commande uniquement si le catalogue de sauvegarde de cet ordinateur est endommagé et que vous n’avez pas de sauvegardes stockées dans un autre emplacement que vous pouvez utiliser pour restaurer le catalogue.|