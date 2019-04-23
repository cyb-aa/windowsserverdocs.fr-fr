---
title: Référence des commandes de sauvegarde Windows Server
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03de0a65-21f0-4dd7-a3ae-251c98bbf6eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ded5039e122832c95eda864bcdcc76f580ca7108
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839810"
---
# <a name="windows-server-backup-command-reference"></a>Référence des commandes de sauvegarde Windows Server



Les éléments suivants sous-commandes pour **wbadmin** fournissent des fonctionnalités de sauvegarde et de récupération à partir d’une invite de commandes.

Pour configurer une planification de sauvegarde, vous devez être un membre de la **administrateurs** groupe. Pour effectuer toutes les autres tâches avec cette commande, vous devez être membre du **opérateurs de sauvegarde** ou le **administrateurs** groupe, ou vous devez vous avoir été délégué des autorisations appropriées.

Vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

|Sous-commande|Description|
|----------|-----------|
|[Activer la sauvegarde WBADMIN](wbadmin-enable-backup.md)|Configure et permet une planification de sauvegarde quotidienne.|
|[désactiver la sauvegarde WBADMIN](wbadmin-disable-backup.md)|Désactive vos sauvegardes quotidiennes.|
|[Démarrer la sauvegarde WBADMIN](wbadmin-start-backup.md)|Exécute une sauvegarde ponctuelle. Si utilisé avec aucun paramètre, utilise les paramètres à partir de la planification de sauvegarde quotidienne.|
|[tâche d’arrêt WBADMIN](wbadmin-stop-job.md)|Arrête l’en cours de sauvegarde ou l’opération de récupération.|
|[WBADMIN get versions](wbadmin-get-versions.md)|Répertorie les détails des sauvegardes récupérables à partir de l’ordinateur local ou, si un autre emplacement est spécifié, à partir d’un autre ordinateur.|
|[WBADMIN get éléments](wbadmin-get-items.md)|Répertorie les éléments inclus dans une sauvegarde spécifique.|
|[WBADMIN start recovery](wbadmin-start-recovery.md)|Exécute une récupération de volumes, des applications, des fichiers ou des dossiers spécifiés.|
|[WBADMIN get état](wbadmin-get-status.md)|Indique l’état de l’en cours de sauvegarde ou l’opération de récupération.|
|[WBADMIN get disques](wbadmin-get-disks.md)|Répertorie les disques qui sont actuellement en ligne.|
|[WBADMIN start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|Exécute une récupération de l’état système.|
|[WBADMIN start systemstatebackup](wbadmin-start-systemstatebackup.md)|Exécute une sauvegarde de l’état système.|
|[WBADMIN delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|Supprime une ou plusieurs sauvegardes d’état système.|
|[WBADMIN start sysrecovery](wbadmin-start-sysrecovery.md)|Exécute une récupération du système complet (au moins tous les volumes qui contiennent l’état du système d’exploitation). La sous-commande est disponible uniquement si vous utilisez l’environnement de récupération Windows.|
|[catalogue de restauration WBADMIN](wbadmin-restore-catalog.md)|Récupère un catalogue de sauvegarde à partir d’un emplacement de stockage spécifié dans le cas où le catalogue de sauvegarde sur l’ordinateur local a été endommagé.|
|[WBADMIN delete catalogue](wbadmin-delete-catalog.md)|Supprime le catalogue de sauvegarde sur l’ordinateur local. Utilisez cette commande uniquement si le catalogue de sauvegarde sur cet ordinateur est endommagé et n’avez aucune sauvegarde stockée dans un autre emplacement que vous pouvez utiliser pour restaurer le catalogue.|