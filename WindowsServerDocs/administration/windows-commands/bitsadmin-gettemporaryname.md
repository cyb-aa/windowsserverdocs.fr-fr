---
title: Bitsadmin gettemporaryname
description: Rubrique de commandes de Windows pour **bitsadmin gettemporaryname** -indique le nom de fichier temporaire du fichier donné au sein du travail.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 762a2a5943202b38e94a245b74745e6631e0792d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876710"
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
|Tâche|Nom d’affichage ou le GUID du travail|
|Fichier d’index|Démarre à 0|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant signale le nom de fichier temporaire du fichier de 2 pour le travail nommé *myJob*.
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)