---
title: bitsadmin getfilestotal
description: La rubrique commandes Windows pour **Bitsadmin getfilestotal**, qui récupère le nombre de fichiers dans le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bad42a8bef57ca4c4a1411a12f20979e4a95d178
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850682"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal

Récupère le nombre de fichiers dans le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getfilestotal <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère le nombre de fichiers inclus dans le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getfilestotal myDownloadJob
```

## <a name="see-also"></a>Voir aussi

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
