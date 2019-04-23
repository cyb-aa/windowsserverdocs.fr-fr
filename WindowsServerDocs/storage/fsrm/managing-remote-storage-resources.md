---
title: Gestion des ressources de stockage distant
description: Cet article explique comment gérer les ressources de stockage sur un ordinateur distant
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 583c36f399848cf67c6f3a850e62015b224768d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836630"
---
# <a name="managing-remote-storage-resources"></a>Gestion des ressources de stockage distant

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Pour gérer les ressources de stockage sur un ordinateur distant, vous avez deux possibilités :

-   Connectez-vous à l’ordinateur distant à partir du composant logiciel enfichable Microsoft<sup>®</sup> Management Console (MMC) Gestionnaire de ressources du serveur de fichiers (que vous pouvez ensuite utiliser pour gérer les ressources distantes).
-   Utilisez les outils en ligne de commande qui sont installés avec le Gestionnaire de ressources du serveur de fichiers.

L'une ou l'autre de ces options vous permet d'utiliser des quotas, de filtrer les fichiers, de gérer les classifications, de planifier les tâches de gestion de fichiers et de générer des rapports avec ces ressources distantes.

> [!Note]
> Le Gestionnaire de ressources du serveur de fichiers peut gérer des ressources soit sur l’ordinateur local, soit sur un ordinateur distant, mais pas les deux en même temps.

Vous pouvez par exemple :

-   vous connecter à un autre ordinateur dans le domaine à l’aide du composant logiciel enfichable MMC Gestionnaire de ressources du serveur de fichiers et vérifier l’utilisation du stockage sur un volume ou un dossier situé sur l’ordinateur distant ;
-   créer des modèles de quota et de filtre de fichiers sur un serveur local, puis utiliser les outils en ligne de commande pour importer ces modèles dans un serveur de fichiers situé dans une filiale.

Cette section comprend les rubriques suivantes :

-   [Se connecter à un ordinateur distant](connect-to-remote-computer.md)
-   [Outils de ligne de commande](command-line-tools.md)
