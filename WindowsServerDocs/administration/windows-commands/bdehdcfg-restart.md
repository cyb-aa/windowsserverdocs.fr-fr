---
title: redémarrage de BdeHdCfg
description: La rubrique commandes Windows pour BdeHdCfg restart-indique à BdeHdCfg que l’ordinateur doit être redémarré une fois la préparation du lecteur terminée.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6c4e48b051f567c98ea679feaa22f995982a899
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382211"
---
# <a name="bdehdcfg-restart"></a>BdeHdCfg : redémarrer



Informe l’outil en ligne de commande BdeHdCfg que l’ordinateur doit être redémarré une fois que la préparation du lecteur est terminée. Pour obtenir un exemple de la façon dont cette commande peut être utilisée, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

### <a name="parameters"></a>Paramètres

Cette commande ne prend pas de paramètres supplémentaires.

## <a name="remarks"></a>Notes

Si d’autres utilisateurs sont connectés à l’ordinateur et que la commande **Quiet** n’est pas spécifiée, une invite s’affiche pour confirmer que l’ordinateur doit être redémarré.

## <a name="BKMK_Examples"></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **restart** .
```
bdehdcfg -target default -restart
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)