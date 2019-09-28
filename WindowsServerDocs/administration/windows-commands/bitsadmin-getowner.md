---
title: bitsadmin getowner
description: La rubrique commandes Windows pour **Bitsadmin GetOwner** -récupère le propriétaire du travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab8151ba8b1379c101aa037504ae2021ff0df62f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381446"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

Affiche le nom complet ou le GUID du propriétaire du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetOwner <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant affiche le propriétaire du travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetOwner myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)