---
title: Infos et bitsadmin cache
description: Rubrique de commandes de Windows pour **bitsadmin cache et informations** -exporte une entrée de cache spécifique.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61ff57b33e575921f2032d4e13a2d9b74accae60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813320"
---
# <a name="bitsadmin-cache-and-info"></a>Infos et bitsadmin cache



Exporte une entrée de cache spécifique.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Cache /Info RecordID [/Verbose] 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|RecordID|Le GUID associé à l’entrée de cache.|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant exporte l’entrée de cache avec la valeur RecordID du {6511FB02-E195-40A2-B595-E8E2F8F47702}.
```
C:\>bitsadmin /Cache /Info {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)