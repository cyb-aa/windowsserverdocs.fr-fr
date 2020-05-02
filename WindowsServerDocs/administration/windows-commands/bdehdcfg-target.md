---
title: cible BdeHdCfg
description: Rubrique de référence pour la commande cible BdeHdCfg, qui prépare une partition à utiliser comme lecteur système par BitLocker et la récupération Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7f98f42675a49ab34ca1cf759efb9d40a69c38a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718595"
---
# <a name="bdehdcfg-target"></a>BdeHdCfg : cible

Prépare une partition à utiliser comme lecteur système par BitLocker et la récupération Windows. Par défaut, cette partition est créée sans lettre de lecteur.

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| default | Indique que l'outil en ligne de commande suivra le même processus que l'Assistant d'installation BitLocker. |
| unallocated | Crée la partition système hors de l'espace non alloué disponible sur le disque. |
| `<drive_letter>`réduire | Réduit le lecteur spécifié selon la quantité nécessaire pour créer une partition système active. Pour utiliser cette commande, le lecteur spécifié doit avoir au moins 5 pour cent d'espace libre. |
| `<drive_letter>`fusion | Utilise le lecteur spécifié comme partition système active. Le lecteur de système d'exploitation ne peut pas être une cible pour la fusion. |

## <a name="examples"></a>Exemples

Pour désigner un lecteur existant (P) comme lecteur système :

```
bdehdcfg -target P: merge
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
