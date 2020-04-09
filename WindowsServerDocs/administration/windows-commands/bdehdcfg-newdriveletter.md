---
title: BdeHdCfg newdriveletter
description: La rubrique commandes Windows pour **BdeHdCfg newdriveletter**, qui affecte une nouvelle lettre de lecteur à la partie d’un lecteur utilisé comme lecteur système.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a8757e7d0684912525817708fbe34953b049582
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851052"
---
# <a name="bdehdcfg-newdriveletter"></a>BdeHdCfg : newdriveletter

Affecte une nouvelle lettre de lecteur à la partie d’un lecteur utilisée comme lecteur système. Pour obtenir un exemple de la façon dont cette commande peut être utilisée, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ---------| ----------- |
|`<DriveLetter>`|Définit la lettre de lecteur qui sera attribuée au lecteur cible spécifié.|

## <a name="remarks"></a>Notes

Il est recommandé de ne pas attribuer de lettre de lecteur à votre lecteur système.

## <a name="examples"></a><a name="BKMK_Examples"></a>Illustre

L’exemple suivant montre le lecteur par défaut auquel la lettre de lecteur P est attribuée.

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)