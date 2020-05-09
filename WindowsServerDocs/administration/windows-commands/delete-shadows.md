---
title: supprimer les ombres
description: Rubrique de référence pour la commande supprimer les ombres, qui supprime les clichés instantanés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b757314c96024741795c6770a98d10ac23b5bd0
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993109"
---
# <a name="delete-shadows"></a>supprimer les ombres

Supprime les clichés instantanés.

## <a name="syntax"></a>Syntaxe

```
delete shadows [all | volume <volume> | oldest <volume> | set <setID> | id <shadowID> | exposed {<drive> | <mountpoint>}]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ---- | ---- |
| all | Supprime tous les clichés instantanés. |
| agrégat`<volume>` | Supprime tous les clichés instantanés du volume donné. |
| plus`<volume>` | Supprime le cliché instantané le plus ancien du volume donné. |
| définie`<setID>` | Supprime les clichés instantanés dans le jeu de clichés instantanés de l’ID donné. Vous pouvez spécifier un alias à l’aide **%** du symbole si l’alias existe dans l’environnement actuel. |
| identifi`<shadowID>` | Supprime un cliché instantané de l’ID donné. Vous pouvez spécifier un alias à l’aide **%** du symbole si l’alias existe dans l’environnement actuel. |
| {'exposé<drive> | <mountpoint>} |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Delete](delete.md)
