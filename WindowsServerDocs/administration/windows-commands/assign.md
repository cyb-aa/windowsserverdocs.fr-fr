---
title: assign
description: Rubrique de référence pour la commande Assign, qui affecte une lettre de lecteur ou un point de montage au volume qui a le focus.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f17c22a0052ade6f16e7842813a04c95e76b57ab
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718985"
---
# <a name="assign"></a>assign

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affecte une lettre de lecteur ou un point de montage au volume qui a le focus. Vous pouvez également utiliser cette commande pour modifier la lettre de lecteur associée à un lecteur amovible. Si aucune lettre de lecteur ou aucun point de montage n’est spécifié, la lettre de lecteur disponible suivante est attribuée. Si la lettre de lecteur ou le point de montage est déjà utilisé, une erreur est générée.

Vous devez sélectionner un volume pour que cette opération aboutisse. Utilisez la commande **Sélectionner un volume** pour sélectionner un volume et lui déplacer le focus.

> [!IMPORTANT]
> Vous ne pouvez pas affecter des lettres de lecteur aux volumes système, aux volumes de démarrage ou aux volumes qui contiennent le fichier d’échange. En outre, vous ne pouvez pas attribuer une lettre de lecteur à une partition OEM (Original Equipment Manufacturer) ou à une partition GPT (GUID partition table) autre qu’une partition de données de base.

## <a name="syntax"></a>Syntaxe

```
assign [{letter=<d> | mount=<path>}] [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `letter=<d>` | Lettre de lecteur que vous souhaitez affecter au volume. |
| `mount=<path>` | Chemin d’accès du point de montage que vous souhaitez affecter au volume. Pour obtenir des instructions sur l’utilisation de cette commande, consultez [assigner un chemin d’accès de dossier de point de montage à un lecteur](https://docs.microsoft.com/windows-server/storage/disk-management/assign-a-mount-point-folder-path-to-a-drive). |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="examples"></a>Exemples

Pour affecter la lettre E au volume sélectionné, tapez :

```
assign letter=e
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande SELECT volume](select-volume.md)