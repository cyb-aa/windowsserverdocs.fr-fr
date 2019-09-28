---
title: Bitsadmin getPriority,
description: La rubrique commandes Windows pour **Bitsadmin getPriority,** -récupère la priorité du travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 0b8914f27c690aa9bb9cbf30430b3edf55f2eb92
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381432"
---
# <a name="bitsadmin-getpriority"></a>Bitsadmin getPriority,

Récupère la priorité du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetPriority <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

La priorité est de **premier plan**, **haute**, **normale**, **faible**ou **inconnue**.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère la priorité pour la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetPriority myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
