---
title: Bitsadmin gettemporaryname
description: La rubrique commandes Windows pour **Bitsadmin gettemporaryname**, qui indique le nom de fichier temporaire du fichier donné au sein du travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c331ecf12cb02d34c76692158c79eafbe5691c5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850452"
---
# <a name="bitsadmin-gettemporaryname"></a>Bitsadmin gettemporaryname

Indique le nom de fichier temporaire du fichier donné au sein du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /gettemporaryname <job> <file_index>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |
| file_index | Démarre à partir de 0. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant indique le nom de fichier temporaire du fichier 2 pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /gettemporaryname myDownloadJob 1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)