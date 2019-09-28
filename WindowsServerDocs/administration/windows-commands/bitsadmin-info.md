---
title: bitsadmin info
description: La rubrique commandes Windows pour **affiche des informations récapitulatives sur le travail spécifié.** -Bitsadmin info
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b6710d73860315fcd13670669871cd310ffb41c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381080"
---
# <a name="bitsadmin-info"></a>bitsadmin info



Affiche des informations récapitulatives sur le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Utilisez le paramètre/Verbose pour fournir des informations détaillées sur le travail.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère des informations sur la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)