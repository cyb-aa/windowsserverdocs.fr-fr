---
title: cache et informations Bitsadmin
description: Rubrique relative aux commandes Windows pour le **cache Bitsadmin et info**, qui vide une entrée de cache spécifique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e9c6ce1eb972a76408483b8a27a3abca5500e56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850892"
---
# <a name="bitsadmin-cache-and-info"></a>cache et informations Bitsadmin

Vide une entrée de cache spécifique.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /cache /info recordID [/verbose]
```

### <a name="parameters"></a>Paramètres

| Paramreter | Description |
| -------------- | -------------- |
| recordID | GUID associé à l’entrée de cache. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant vide l’entrée de cache avec la valeur recordID {6511FB02-E195-40A2-B595-E8E2F8F47702}.

```
C:\>bitsadmin /cache /info {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)