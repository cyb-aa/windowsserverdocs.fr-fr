---
title: taille de BdeHdCfg
description: 'Rubrique relative aux commandes Windows : spécifie la taille de la partition système lors de la création d’un nouveau lecteur système.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ec42cdb5716c63c7210ea6cfde8ce8884833b45
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382197"
---
# <a name="bdehdcfg-size"></a>BdeHdCfg : taille



Spécifie la taille de la partition système lors de la création d’un nouveau lecteur système. Pour obtenir un exemple de la façon dont cette commande peut être utilisée, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<SizeinMB >|Indique le nombre de mégaoctets (Mo) à utiliser pour la nouvelle partition.|

## <a name="remarks"></a>Notes

Si vous ne spécifiez pas de taille, l'outil utilisera la valeur par défaut de 300 Mo La taille minimale du lecteur système est 100 Mo Si vous stockez les outils de récupération système ou d'autres outils système sur la partition système, vous devez augmenter la taille en conséquence.

> [!NOTE]
> La commande **Size** ne peut pas être combinée avec la **commande \<lettre_lecteur** > **Merge** .

## <a name="BKMK_Examples"></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **Size** pour allouer 500 Mo au lecteur système par défaut.
```
bdehdcfg -target default -size 500
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)