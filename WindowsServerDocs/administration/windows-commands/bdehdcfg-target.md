---
title: cible BdeHdCfg
description: La rubrique commandes Windows pour la **cible BdeHdCfg**, qui prépare une partition à utiliser comme lecteur système par BitLocker et la récupération Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0f3e90fbb8725360cf8db335e79721e2328ab3a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851012"
---
# <a name="bdehdcfg-target"></a>BdeHdCfg : cible

Prépare une partition à utiliser comme lecteur système par BitLocker et la récupération Windows. Par défaut, cette partition est créée sans lettre de lecteur.

Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| default | Indique que l'outil en ligne de commande suivra le même processus que l'Assistant d'installation BitLocker. |
| unallocated | Crée la partition système hors de l'espace non alloué disponible sur le disque. |
| réduire `<DriveLetter>` | Réduit le lecteur spécifié selon la quantité nécessaire pour créer une partition système active. Pour utiliser cette commande, le lecteur spécifié doit avoir au moins 5 pour cent d'espace libre. |
| fusion `<DriveLetter>` | Utilise le lecteur spécifié comme partition système active. Le lecteur de système d'exploitation ne peut pas être une cible pour la fusion. |

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **target** pour désigner un lecteur existant (P) comme lecteur système.

```
bdehdcfg -target P: merge
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)