---
title: BdeHdCfg newdriveletter
description: La rubrique commandes Windows pour BdeHdCfg newdriveletter-attribue une nouvelle lettre de lecteur à la partie d’un lecteur utilisée comme lecteur système.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2abd4a686f358b5dd844514735edb3ffaa13845
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382228"
---
# <a name="bdehdcfg-newdriveletter"></a>BdeHdCfg : newdriveletter



Affecte une nouvelle lettre de lecteur à la partie d’un lecteur utilisée comme lecteur système. Pour obtenir un exemple de la façon dont cette commande peut être utilisée, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0DriveLetter >|Définit la lettre de lecteur qui sera attribuée au lecteur cible spécifié.|

## <a name="remarks"></a>Notes

Il est recommandé de ne pas attribuer de lettre de lecteur à votre lecteur système.

## <a name="BKMK_Examples"></a>Illustre

L’exemple suivant montre le lecteur par défaut auquel la lettre de lecteur P est attribuée.
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)