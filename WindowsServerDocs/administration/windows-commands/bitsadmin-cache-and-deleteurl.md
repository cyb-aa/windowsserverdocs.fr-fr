---
title: cache Bitsadmin et DeleteUrl
description: La rubrique commandes Windows pour le **cache Bitsadmin et DeleteUrl** -supprime toutes les entrées de cache pour l’URL donnée.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 869d3bc0f011cc82aaea9b7468667964051e1c00
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382059"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>cache Bitsadmin et DeleteUrl



Supprime toutes les entrées de cache pour l’URL donnée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /DeleteURL url
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|url|Uniform Resource Locator qui identifie un fichier distant.|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant supprime toutes les entrées de cache pour https://www.microsoft.com/en/us/default.aspx
```
C:\>bitsadmin /DeleteURL https://www.microsoft.com/en/us/default.aspx 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)