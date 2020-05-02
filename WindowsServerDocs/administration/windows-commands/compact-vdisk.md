---
title: Compact vdisk
description: Rubrique de référence pour la commande compact vdisk, qui réduit la taille physique d’un fichier de disque dur virtuel (VHD) de taille dynamique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4ae5c653645c9f6f3ef97501a59932682c24be3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82711150"
---
# <a name="compact-vdisk"></a>Compact vdisk

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Réduit la taille physique d’un fichier de disque dur virtuel (VHD) de taille dynamique. Ce paramètre est utile, car la taille des disques durs virtuels à extension dynamique augmente au fur et à mesure que vous ajoutez des fichiers, mais ils ne diminuent pas automatiquement la taille lorsque vous supprimez des fichiers.

## <a name="syntax"></a>Syntaxe

```
compact vdisk
```

### <a name="remarks"></a>Notes

- Un VHD de taille dynamique doit être sélectionné pour que cette opération aboutisse. Utilisez la [commande SELECT vdisk](select-vdisk.md) pour sélectionner un disque dur virtuel et lui déplacer le focus.

- Vous pouvez uniquement utiliser des disques durs virtuels compacts de taille dynamique qui sont détachés ou attachés en lecture seule.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Attach vdisk](attach-vdisk.md)

- [commande de détails vdisk](detail-vdisk.md)

- [Commande Detach vdisk](detach-vdisk.md)

- [commande Expand vdisk](expand-vdisk.md)

- [Commande Merge vdisk](merge-vdisk.md)

- [sélectionner la commande vdisk](select-vdisk.md)

- [liste, commande](list.md)
