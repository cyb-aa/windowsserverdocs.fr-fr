---
title: Bitsadmin util et enableanalyticchannel
description: La rubrique commandes Windows pour **Bitsadmin util et enableanalyticchannel**, qui active ou désactive le canal analytique du client bits.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8ff1f835415979036fdc0f8aa637fe693e57d46
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122682"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>Bitsadmin util et enableanalyticchannel

Active ou désactive le canal analytique du client BITS.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| Paramètre | Description |
| --------- | ---------- |
| TRUE ou FALSe | **True** active la validation du contenu pour le fichier spécifié, tandis que **false** le désactive. |

## <a name="examples"></a>Exemples

L’exemple suivant active le canal analytique du client BITS.

```
C:\>bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)