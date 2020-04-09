---
title: bitsadmin getcompletiontime
description: La rubrique commandes Windows pour **Bitsadmin getcompletiontime**, qui récupère l’heure à laquelle le travail a fini de transférer des données.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5408e7e8c35135601a4a0af0ab7e9c55cea4c8dd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850752"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime

Récupère l’heure à laquelle le travail a terminé le transfert des données.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getcompletiontime <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère l’heure à laquelle le travail nommé *myDownloadJob* a terminé le transfert des données.

```
C:\>bitsadmin /getcompletiontime myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)