---
title: Bitsadmin gettemporaryname
description: La rubrique commandes Windows pour **Bitsadmin gettemporaryname** -indique le nom de fichier temporaire du fichier donné au sein du travail.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b665fae4c0bfdd5ea04b929be49f9590430b358
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381301"
---
# <a name="bitsadmin-gettemporaryname"></a>Bitsadmin gettemporaryname



Indique le nom de fichier temporaire du fichier donné au sein du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetTemporaryName <Job> <file index> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Index de fichiers|Commence à 0|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant indique le nom de fichier temporaire du fichier 2 pour le travail nommé *myJob*.
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)