---
title: bitsadmin util and enableanalyticchannel
description: Rubrique de référence pour la commande Bitsadmin util et enableanalyticchannel, qui active ou désactive le canal analytique du client BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1f5c8c924d1011928aca6ec1bcebd4d71abb015
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707760"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>bitsadmin util and enableanalyticchannel

Active ou désactive le canal analytique du client BITS.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| Paramètre | Description |
| --------- | ---------- |
| TRUE ou FALSe | **True** active la validation du contenu pour le fichier spécifié, tandis que **false** le désactive. |

## <a name="examples"></a>Exemples

Pour activer ou désactiver le canal analytique du client BITS.

```
bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin util](bitsadmin-util.md)

- [commande Bitsadmin](bitsadmin.md)
