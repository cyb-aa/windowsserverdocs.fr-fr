---
title: bitsadmin cache and info
description: Rubrique de référence pour le cache Bitsadmin et la commande info, qui vide une entrée de cache spécifique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a50e6575a5496ff9f7bcd6a0dc429c7960c6933
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718349"
---
# <a name="bitsadmin-cache-and-info"></a>bitsadmin cache and info

Vide une entrée de cache spécifique.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /cache /info recordID [/verbose]
```

### <a name="parameters"></a>Paramètres

| Paramreter | Description |
| -------------- | -------------- |
| recordID | GUID associé à l’entrée de cache. |

## <a name="examples"></a>Exemples

Pour vider l’entrée du cache avec la valeur recordID {6511FB02-E195-40A2-B595-E8E2F8F47702} :

```
bitsadmin /cache /info {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande de cache Bitsadmin](bitsadmin-cache.md)
