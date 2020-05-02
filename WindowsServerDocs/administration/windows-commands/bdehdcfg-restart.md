---
title: redémarrage de BdeHdCfg
description: Rubrique de référence pour la commande BdeHdCfg Restart, qui indique à BdeHdCfg que l’ordinateur doit être redémarré une fois la préparation du lecteur terminée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 684a6a24fe78c0a23ba954981121c7bd99ac56fb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718625"
---
# <a name="bdehdcfg-restart"></a>BdeHdCfg : redémarrer

Informe l’outil en ligne de commande BdeHdCfg que l’ordinateur doit être redémarré une fois que la préparation du lecteur est terminée. Si d’autres utilisateurs sont connectés à l’ordinateur et que la commande **Quiet** n’est pas spécifiée, une invite s’affiche pour confirmer que l’ordinateur doit être redémarré.

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -restart
```

#### <a name="parameters"></a>Paramètres

Cette commande n’a pas de paramètres supplémentaires.

## <a name="examples"></a>Exemples

Pour utiliser la commande **restart** :

```
bdehdcfg -target default -restart
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
