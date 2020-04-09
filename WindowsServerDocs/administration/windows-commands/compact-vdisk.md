---
title: Compact vdisk
description: Windows Commands topic forcompact vdisk, qui réduit la taille physique d’un fichier de disque dur virtuel (VHD) de taille dynamique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9691be21c188fbc2c3b2e782acde127270decf56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847422"
---
# <a name="compact-vdisk"></a>Compact vdisk

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Réduit la taille physique d’un fichier de disque dur virtuel (VHD) de taille dynamique. Ce paramètre est utile, car la taille des disques durs virtuels à extension dynamique augmente au fur et à mesure que vous ajoutez des fichiers, mais ils ne diminuent pas automatiquement la taille lorsque vous supprimez des fichiers.

> [!NOTE]
> Cette commande s’applique uniquement à Windows 7 et Windows Server 2008 R2.

## <a name="syntax"></a>Syntaxe
```
compact vdisk
```

## <a name="remarks"></a>Notes

- Un VHD de taille dynamique doit être sélectionné pour que cette opération aboutisse. Utilisez la commande **Select vdisk** pour sélectionner un disque dur virtuel et lui déplacer le focus.

- Vous ne pouvez compacter que des disques durs virtuels de taille dynamique qui sont détachés ou attachés en lecture seule.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
Pour compacter un disque dur virtuel de taille dynamique, tapez :
```
compact vdisk
```

## <a name="additional-references"></a>Références supplémentaires
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
- [attacher vdisk](attach-vdisk.md)
- [détailler vdisk](detail-vdisk.md)
- [Détacher vdisk](detach-vdisk.md)
- [développer vdisk](expand-vdisk.md)
- [Merge vdisk](merge-vdisk.md)
- [sélectionner vdisk](select-vdisk.md)
- [list_1](list_1.md)
