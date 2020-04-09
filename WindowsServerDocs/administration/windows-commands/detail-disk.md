---
title: disque de détail
description: La rubrique commandes Windows pour le disque de détail, qui affiche les propriétés du disque sélectionné et les volumes sur ce disque.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d0768d45c0f56ba549ff54064c4e74ae3048e41
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846452"
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

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour afficher les propriétés du disque sélectionné, ainsi que des informations sur les volumes du disque, tapez :
```
detail disk
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

