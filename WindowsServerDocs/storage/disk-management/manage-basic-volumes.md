---
title: Gérer les volumes de base
description: Cet article explique comment gérer les volumes de base.
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c75d887a6427673319999522b890d523f4276871
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870960"
---
# <a name="manage-basic-volumes"></a>Gérer les volumes de base

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un disque de base est un disque physique qui contient des partitions principales, des partitions étendues ou des lecteurs logiques. Les partitions et les lecteurs logiques de disques de base sont appelés volumes de base. Vous pouvez uniquement créer des volumes de base sur les disques de base.

Vous pouvez ajouter davantage d’espace à des partitions principales et des lecteurs logiques existants en les étendant dans un espace non alloué contigu et adjacent sur le même disque. Pour étendre un volume de base, il doit être au format de système de fichiers NTFS. Vous pouvez étendre un lecteur logique dans un espace libre contigu dans la partition étendue qui le contient. Si vous étendez un lecteur logique au-delà de l’espace libre disponible dans la partition étendue, la partition étendue s’agrandit pour contenir le lecteur logique si la partition étendue est suivie d’un espace non alloué contigu.

## <a name="see-also"></a>Voir aussi

-   [Affecter un chemin d’accès du dossier de point de montage à un lecteur](assign-a-mount-point-folder-path-to-a-drive.md)
-   [Étendre un Volume de base](extend-a-basic-volume.md)
-   [Réduire un Volume de base](shrink-a-basic-volume.md)