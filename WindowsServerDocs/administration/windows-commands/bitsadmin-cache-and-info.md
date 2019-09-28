---
title: cache et informations Bitsadmin
description: La rubrique commandes Windows pour le **cache Bitsadmin et info** -vide une entrée de cache spécifique.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 11963ff5640ef30e597e5e802778aff121c0efb3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381967"
---
# <a name="bitsadmin-cache-and-info"></a>cache et informations Bitsadmin



Vide une entrée de cache spécifique.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Cache /Info RecordID [/Verbose] 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|RecordID|GUID associé à l’entrée de cache.|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant vide l’entrée de cache avec la valeur RecordID de {6511FB02-E195-40A2-B595-E8E2F8F47702}.
```
C:\>bitsadmin /Cache /Info {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)