---
title: bitsadmin getbytestransferred
description: La rubrique commandes Windows pour **Bitsadmin getbytestransferred**, qui récupère le nombre d’octets transférés pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 957b3e60bf8a5e41b3964f4d762633472606654d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850772"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred

Récupère le nombre d’octets transférés pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getbytestransferred <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère le nombre d’octets transférés pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getbytestransferred myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)