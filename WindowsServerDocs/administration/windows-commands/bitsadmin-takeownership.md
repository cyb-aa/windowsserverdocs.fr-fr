---
title: bitsadmin takeownership
description: La rubrique commandes Windows pour **Bitsadmin TakeOwnership** -permet à un utilisateur disposant de privilèges d’administrateur d’assumer la propriété du travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f0d0610b2ba6437f6fdd41bf1b875993cf11f2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380358"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership



Permet à un utilisateur disposant de privilèges d’administrateur d’assumer la propriété du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /TakeOwnership <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant prend la propriété de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /TakeOwnership myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)