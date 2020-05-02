---
title: BdeHdCfg newdriveletter
description: Rubrique de référence pour la commande BdeHdCfg newdriveletter, qui affecte une nouvelle lettre de lecteur à la partie d’un lecteur utilisée comme lecteur système.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da09ae1469c6fc8370e6bd0f2f7a8f3efd8dc4f0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718665"
---
# <a name="bdehdcfg-newdriveletter"></a>BdeHdCfg : newdriveletter

Affecte une nouvelle lettre de lecteur à la partie d’un lecteur utilisée comme lecteur système. Il est recommandé de ne pas attribuer de lettre de lecteur à votre lecteur système.

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -newdriveletter <drive_letter>
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ---------| ----------- |
| `<drive_letter>` | Définit la lettre de lecteur qui sera attribuée au lecteur cible spécifié. |

## <a name="examples"></a>Exemples

Pour affecter la lettre `P`de lecteur par défaut :

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
