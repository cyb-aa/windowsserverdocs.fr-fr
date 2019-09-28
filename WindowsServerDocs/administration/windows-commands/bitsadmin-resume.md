---
title: bitsadmin resume
description: La rubrique commandes Windows pour **Bitsadmin Resume** -active un travail nouveau ou suspendu dans la file d’attente de transfert.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1393e959980b72de09c546ced763a506d334b56c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380770"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume



Active une tâche nouvelle ou suspendue dans la file d’attente de transfert.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Resume <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant reprend la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /Resume myDownloadJob
```
Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)