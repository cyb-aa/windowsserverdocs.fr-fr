---
title: DeleteUrl et bitsadmin cache
description: Rubrique de commandes de Windows pour **bitsadmin cache et deleteurl** -supprime toutes les entrées de cache pour l’URL donnée.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a831c49e1461761cb7466b46e7a5ad8e037f4ec9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816650"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>DeleteUrl et bitsadmin cache



Supprime toutes les entrées de cache pour l’URL donnée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /DeleteURL url
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|url|L’URL qui identifie un fichier distant.|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant supprime toutes les entrées de cache pour https://www.microsoft.com/en/us/default.aspx
```
C:\>bitsadmin /DeleteURL https://www.microsoft.com/en/us/default.aspx 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)