---
title: cache Bitsadmin et setLimit
description: La rubrique commandes Windows pour le **cache Bitsadmin et setLimit** -définit la limite de taille du cache.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88a10ce8599202e237daa6822cf62806d3c21429
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381942"
---
# <a name="bitsadmin-cache-and-setlimit"></a>cache Bitsadmin et setLimit



Définit la limite de taille du cache.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Cache /SetLimit Percent
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Percent|Limite du cache définie sous la forme d’un pourcentage de l’espace total du disque dur.|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant limite la taille du cache à 50%.
```
C:\>bitsadmin /Cache /SetLimit 50 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)