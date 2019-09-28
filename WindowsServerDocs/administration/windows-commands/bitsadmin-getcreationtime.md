---
title: bitsadmin getcreationtime
description: La rubrique commandes Windows pour **Bitsadmin GetCreationTime** -récupère l’heure de création du travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ea92133c90e20e37e5d281116e91bf1f109e83f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381687"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime



Récupère l’heure de création du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetCreationTime <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère l’heure de création de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetCreationTime myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)