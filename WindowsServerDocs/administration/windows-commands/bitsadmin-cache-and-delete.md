---
title: suppression et bitsadmin cache
description: Rubrique de commandes de Windows pour **bitsadmin mettre en cache et supprimer** -supprime une entrée de cache spécifique.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63b82cbbadebf2c4e36f2c76076b329787d7b1b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852510"
---
# <a name="bitsadmin-cache-and-delete"></a>suppression et bitsadmin cache



Supprime une entrée de cache spécifique.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Cache /Delete RecordID 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|RecordID|Le GUID associé à l’entrée de cache.|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant supprime l’entrée de cache avec la valeur RecordID du {6511FB02-E195-40A2-B595-E8E2F8F47702}.
```
C:\>bitsadmin /Cache /Delete {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)