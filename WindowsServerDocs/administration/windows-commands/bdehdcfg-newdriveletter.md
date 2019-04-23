---
title: bdehdcfg newdriveletter
description: Rubrique de commandes de Windows pour bdehdcfg newdriveletter - assigne une nouvelle lettre de lecteur à la partie d’un lecteur utilisé comme lecteur système.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cd40942dfb724d46c0fa9a43c4646e1db09d2a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887140"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg: newdriveletter



Affecte une nouvelle lettre de lecteur à la partie d’un lecteur utilisé comme lecteur système. Pour obtenir un exemple d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -newdriveletter <DriveLetter>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<DriveLetter>|Définit la lettre de lecteur qui sera affectée au lecteur cible spécifié.|

## <a name="remarks"></a>Notes

Comme meilleure pratique, il est recommandé que vous n’affectez pas une lettre de lecteur sur votre lecteur système.

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant montre le lecteur par défaut assigné la lettre de lecteur P.
```
bdehdcfg -target default -newdriveletter P:
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)