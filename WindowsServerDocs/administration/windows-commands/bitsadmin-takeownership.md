---
title: bitsadmin takeownership
description: Rubrique de commandes de Windows pour **bitsadmin takeownership** -permet à un utilisateur avec des privilèges d’administrateur de prendre possession de la tâche spécifiée.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: aedca49e43588ab51f84477cf8690cf58486c3cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827840"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership



Permet à un utilisateur avec des privilèges d’administrateur de prendre possession de la tâche spécifiée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /TakeOwnership <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant prend possession de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /TakeOwnership myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)