---
title: taille de bdehdcfg
description: Rubrique de commandes de Windows - spécifie la taille de la partition système lorsqu’un nouveau lecteur système est créé.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d024bb4092f93782300d6afb9053cee1da32629a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817520"
---
# <a name="bdehdcfg-size"></a>bdehdcfg: size



Spécifie la taille de la partition système lorsqu’un nouveau lecteur système est créé. Pour obtenir un exemple d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<SizeinMB>|Indique le nombre de mégaoctets (Mo) à utiliser pour la nouvelle partition.|

## <a name="remarks"></a>Notes

Si vous ne spécifiez pas de taille, l'outil utilisera la valeur par défaut de 300 Mo La taille minimale du lecteur système est 100 Mo Si vous stockez les outils de récupération système ou d'autres outils système sur la partition système, vous devez augmenter la taille en conséquence.

> [!NOTE]
> Le **taille** commande ne peut pas être combinée avec la **cible** \<lettre_lecteur > **fusion** commande.

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant illustre l’utilisation de la **taille** commande allouer 500 Mo sur le lecteur du système par défaut.
```
bdehdcfg -target default -size 500
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)