---
title: redémarrage de BdeHdCfg
description: La rubrique commandes Windows pour **BdeHdCfg restart**, qui indique à BdeHdCfg que l’ordinateur doit être redémarré une fois la préparation du lecteur terminée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ae6f8d31c09feddf8f994c28d34e4e1b08cc322
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851032"
---
# <a name="bdehdcfg-restart"></a>BdeHdCfg : redémarrer

Informe l’outil en ligne de commande BdeHdCfg que l’ordinateur doit être redémarré une fois que la préparation du lecteur est terminée. Pour obtenir un exemple de la façon dont cette commande peut être utilisée, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

#### <a name="parameters"></a>Paramètres

Cette commande ne prend pas de paramètres supplémentaires.

## <a name="remarks"></a>Notes

Si d’autres utilisateurs sont connectés à l’ordinateur et que la commande **Quiet** n’est pas spécifiée, une invite s’affiche pour confirmer que l’ordinateur doit être redémarré.

## <a name="examples"></a><a name="BKMK_Examples"></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **restart** .

```
bdehdcfg -target default -restart
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)