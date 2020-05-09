---
title: detach vdisk
description: Rubrique de référence pour la commande Detach vdisk, qui empêche le disque dur virtuel (VHD) sélectionné d’apparaître comme un lecteur de disque dur local sur l’ordinateur hôte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 427b27630341589f3ff6dd422667e1247f5b64ec
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993084"
---
# <a name="detach-vdisk"></a>detach vdisk

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arrête l’affichage du disque dur virtuel sélectionné en tant que lecteur de disque dur local sur l’ordinateur hôte. Lorsqu'un VHD est détaché, vous pouvez le copier à d'autres emplacements. Avant de commencer, vous devez sélectionner un disque dur virtuel pour que cette opération aboutisse. Utilisez la commande [Select vdisk](select-vdisk.md) pour sélectionner un disque dur virtuel et lui déplacer le focus.


## <a name="syntax"></a>Syntaxe

```
detach vdisk [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="examples"></a>Exemples

Pour détacher le disque dur virtuel sélectionné, tapez :

```
detach vdisk
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Attach vdisk](attach-vdisk.md)

- [commande compact vdisk](compact-vdisk.md)

- [commande de détails vdisk](detail-vdisk.md)

- [commande Expand vdisk](expand-vdisk.md)

- [Commande Merge vdisk](merge-vdisk.md)

- [sélectionner la commande vdisk](select-vdisk.md)

- [liste, commande](list.md)
