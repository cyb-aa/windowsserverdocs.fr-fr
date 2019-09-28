---
title: bitsadmin getfilestransferred
description: La rubrique commandes Windows pour **Bitsadmin getfilestransferred** -récupère le nombre de fichiers transférés pour le travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d02d9d7bc216a5ad7ca922e716c368f64c4b9a44
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381600"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred



Récupère le nombre de fichiers transférés pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetFilesTransferred <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère le nombre de fichiers transférés dans le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTransferred myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)