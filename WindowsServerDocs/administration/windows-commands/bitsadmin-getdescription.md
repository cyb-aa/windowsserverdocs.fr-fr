---
title: bitsadmin getdescription
description: Rubrique relative aux commandes Windows pour **Bitsadmin GetDescription** -récupère la description du travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 02ab91ad9b6d1d6d1ef67465bb5c982fbddc1bb4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381647"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription



Récupère la description du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetDescription <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère la description de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetDescription myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)