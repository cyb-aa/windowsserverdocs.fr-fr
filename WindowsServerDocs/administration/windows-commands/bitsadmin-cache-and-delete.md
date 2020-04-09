---
title: cache Bitsadmin et suppression
description: Rubrique relative aux commandes Windows pour **Bitsadmin cache et Delete**, qui supprime une entrée de cache spécifique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fd7f1db83a62dd9c1085d6afdcf509c1c3ac8cf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850942"
---
# <a name="bitsadmin-cache-and-delete"></a>cache Bitsadmin et suppression

Supprime une entrée de cache spécifique.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /cache /delete recordID
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| recordID | GUID associé à l’entrée de cache. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant supprime l’entrée de cache avec la valeur RecordID de {6511FB02-E195-40A2-B595-E8E2F8F47702}.

```
C:\>bitsadmin /cache /delete {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)