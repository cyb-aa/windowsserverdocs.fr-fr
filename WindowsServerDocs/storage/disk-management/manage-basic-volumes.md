---
title: Gérer les volumes de base
description: Cet article explique comment gérer les volumes de base.
ms.date: 10/12/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2eee5820891c108475c58c9ad024cd7c00391e43
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385924"
---
# <a name="manage-basic-volumes"></a>Gérer les volumes de base

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (Canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un disque de base est un disque physique qui contient des partitions principales, des partitions étendues ou des lecteurs logiques. Les partitions et les lecteurs logiques de disques de base sont appelés volumes de base. Vous pouvez créer des volumes de base uniquement sur les disques de base.

Vous pouvez ajouter plus d’espace à des partitions principales et des lecteurs logiques existants en les étendant dans un espace non alloué contigu et adjacent sur le même disque. Pour étendre un volume de base, celui-ci doit être formaté avec le système de fichiers NTFS. Vous pouvez étendre un lecteur logique dans un espace libre contigu, dans la partition étendue qui le contient. Si vous étendez un lecteur logique au-delà de l’espace libre disponible dans la partition étendue, cette partition étendue s’agrandit pour contenir le lecteur logique à condition qu’elle soit suivie d’un espace non alloué contigu.

## <a name="see-also"></a>Voir aussi

-   [Attribuer un chemin d’accès de dossier de point de montage à un lecteur](assign-a-mount-point-folder-path-to-a-drive.md)
-   [Étendre un volume de base](extend-a-basic-volume.md)
-   [Réduire un volume de base](shrink-a-basic-volume.md)