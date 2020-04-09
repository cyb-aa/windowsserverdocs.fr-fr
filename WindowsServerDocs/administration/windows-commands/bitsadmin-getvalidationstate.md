---
title: Bitsadmin getvalidationstate
description: La rubrique commandes Windows pour **Bitsadmin getvalidationstate**, qui indique l’état de validation du contenu du fichier donné au sein du travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52d7d983cc7858607c350483ed81223d107cee25
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850432"
---
# <a name="bitsadmin-getvalidationstate"></a>Bitsadmin getvalidationstate

Signale l’état de validation du contenu du fichier donné au sein du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getvalidationstate <job> <file_index>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |
| file_index | Démarre à partir de 0. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant obtient l’état de validation du contenu du fichier 2 au sein du travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getvalidationstate myDownloadJob 1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)