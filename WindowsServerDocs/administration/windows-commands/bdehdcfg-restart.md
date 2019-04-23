---
title: redémarrage de bdehdcfg
description: 'Rubrique de commandes de Windows pour bdehdcfg restart : indique que l’ordinateur doit être redémarré après la préparation du lecteur de la bdehdcfg.'
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0f361db8fdf33bd414556575de75241f7dbd9327
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879460"
---
# <a name="bdehdcfg-restart"></a>Bdehdcfg : redémarrer



Informe l’outil de ligne de commande Bdehdcfg que l’ordinateur doit être redémarré après la préparation du lecteur. Pour obtenir un exemple d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

### <a name="parameters"></a>Paramètres

Cette commande n’accepte aucuns paramètres supplémentaires.

## <a name="remarks"></a>Notes

Si d’autres utilisateurs sont connectés à l’ordinateur et le **silencieux** commande n’est pas spécifiée, une invite s’affiche pour confirmer que l’ordinateur doit être redémarré.

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant illustre l’utilisation de la **redémarrer** commande.
```
bdehdcfg -target default -restart
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)