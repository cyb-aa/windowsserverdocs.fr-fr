---
title: disque de détail
description: Rubrique de référence sur le disque de détail, qui affiche les propriétés du disque sélectionné et les volumes sur ce disque.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a746506d6c9609e3214dbd48e5fa91f52d16ab4d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82710517"
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

## <a name="examples"></a>Exemples

Pour afficher les propriétés du disque sélectionné, ainsi que des informations sur les volumes du disque, tapez :
```
detail disk
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

