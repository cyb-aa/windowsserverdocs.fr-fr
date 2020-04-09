---
title: taille de BdeHdCfg
description: La rubrique commandes Windows pour **BdeHdCfg Size**, qui spécifie la taille de la partition système lors de la création d’un nouveau lecteur système.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c72bdfd0b8bf4dfd0aa36885ceb7fd249a18c332
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851022"
---
# <a name="bdehdcfg-size"></a>BdeHdCfg : taille

Spécifie la taille de la partition système lors de la création d’un nouveau lecteur système.

Pour obtenir un exemple de la façon dont cette commande peut être utilisée, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<SizeinMB>` | Indique le nombre de mégaoctets (Mo) à utiliser pour la nouvelle partition. |

## <a name="remarks"></a>Notes

Si vous ne spécifiez pas de taille, l'outil utilisera la valeur par défaut de 300 Mo La taille minimale du lecteur système est 100 Mo Si vous stockez les outils de récupération système ou d'autres outils système sur la partition système, vous devez augmenter la taille en conséquence.

> [!NOTE]
> La commande **Size** ne peut pas être combinée avec la commande `<DriveLetter>` de **fusion** **cible** .

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **Size** pour allouer 500 Mo au lecteur système par défaut.

```
bdehdcfg -target default -size 500
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)