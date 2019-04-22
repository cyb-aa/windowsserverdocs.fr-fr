---
title: disque de détail
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c7a5063edf3cb2e190e8aec957e1b571c1f15bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819100"
---
# <a name="detail-disk"></a>disque de détail



Affiche les propriétés du disque sélectionné et des volumes figurant sur le disque.

## <a name="syntax"></a>Syntaxe

```
detail disk
```

## <a name="remarks"></a>Notes

-   Un disque doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque et de déplacer le focus vers elle.
-   Si le disque sélectionné est un disque dur virtuel (VHD), **disque de détail** signale le type de bus du disque en tant que virtuelle.

## <a name="BKMK_examples"></a>Exemples

Pour afficher les propriétés de disque sélectionné, les informations sur les volumes sur le disque, tapez :
```
detail disk
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

