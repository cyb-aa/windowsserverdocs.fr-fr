---
title: bitsadmin cache and setlimit
description: Rubrique de référence pour le cache Bitsadmin et la commande setLimit, qui définit la limite de taille du cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4c41102bfb87ff6d48113c4e85a821b821b5b01
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718289"
---
# <a name="bitsadmin-cache-and-setlimit"></a>bitsadmin cache and setlimit

Définit la limite de taille du cache.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /cache /setlimit percent
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| pour cent | Limite du cache définie sous la forme d’un pourcentage de l’espace total du disque dur. |

## <a name="examples"></a>Exemples

Pour définir la limite de taille du cache à 50% :

```
bitsadmin /cache /setlimit 50
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande de cache Bitsadmin](bitsadmin-cache.md)
