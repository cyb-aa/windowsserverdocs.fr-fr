---
title: bitsadmin cache and delete
description: Rubrique de référence pour le cache Bitsadmin et la commande Delete, qui supprime une entrée de cache spécifique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62c0c3d5b2cc188e8a8987c7ca502cdeaf932410
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718460"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin cache and delete

Supprime une entrée de cache spécifique.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /cache /delete recordID
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| recordID | GUID associé à l’entrée de cache. |

## <a name="examples"></a>Exemples

Pour supprimer l’entrée de cache avec la valeur RecordID de {6511FB02-E195-40A2-B595-E8E2F8F47702} :

```
bitsadmin /cache /delete {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande de cache Bitsadmin](bitsadmin-cache.md)
