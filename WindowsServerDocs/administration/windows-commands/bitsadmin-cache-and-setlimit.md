---
title: setlimit et bitsadmin cache
description: Rubrique de commandes de Windows pour **bitsadmin cache et setlimit** -définit la limite de taille de cache.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d0b72c5ec6c779fa4ce3fa038352836cd9456ac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852590"
---
# <a name="bitsadmin-cache-and-setlimit"></a>setlimit et bitsadmin cache



Définit la limite de taille de cache.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Cache /SetLimit Percent
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|%|La limite de cache définie sous forme de pourcentage de l’espace disque total...|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant limite la taille du cache à 50 %.
```
C:\>bitsadmin /Cache /SetLimit 50 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)