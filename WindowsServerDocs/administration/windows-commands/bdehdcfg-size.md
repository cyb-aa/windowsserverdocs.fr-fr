---
title: taille de BdeHdCfg
description: Rubrique de référence pour la commande BdeHdCfg Size, qui spécifie la taille de la partition système lors de la création d’un nouveau lecteur système.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27662a46045a8701b40697198cddf1dcb1b51c1e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718612"
---
# <a name="bdehdcfg-size"></a>BdeHdCfg : taille

Spécifie la taille de la partition système lors de la création d’un nouveau lecteur système. Si vous ne spécifiez pas de taille, l'outil utilisera la valeur par défaut de 300 Mo La taille minimale du lecteur système est 100 Mo Si vous stockez les outils de récupération système ou d'autres outils système sur la partition système, vous devez augmenter la taille en conséquence.

> [!NOTE]
> La commande **Size** ne peut pas être combinée `target <drive_letter> merge` avec la commande.

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink} -size <size_in_mb>
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<size_in_mb>` | Indique le nombre de mégaoctets (Mo) à utiliser pour la nouvelle partition. |

## <a name="examples"></a>Exemples

Pour allouer 500 Mo sur le lecteur système par défaut :

```
bdehdcfg -target default -size 500
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
