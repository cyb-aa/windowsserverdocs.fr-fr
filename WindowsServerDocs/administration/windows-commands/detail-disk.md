---
title: disque de détail
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ff78a3f9e27cde35a7e19bdf1565c515a127261b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378577"
---
# <a name="detail-disk"></a>disque de détail



Affiche les propriétés du disque sélectionné et des volumes figurant sur le disque.

## <a name="syntax"></a>Syntaxe

```
detail disk
```

## <a name="remarks"></a>Notes

-   Vous devez sélectionner un disque pour que cette opération aboutisse. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.
-   Si le disque sélectionné est un disque dur virtuel (VHD), le **disque de détail** signale le type de bus du disque comme étant virtuel.

## <a name="BKMK_examples"></a>Illustre

Pour afficher les propriétés du disque sélectionné, ainsi que des informations sur les volumes du disque, tapez :
```
detail disk
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

