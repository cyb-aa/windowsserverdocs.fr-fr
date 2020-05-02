---
title: bitsadmin gettemporaryname
description: Rubrique de référence pour la commande Bitsadmin gettemporaryname, qui indique le nom de fichier temporaire du fichier donné au sein du travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f7780691f37fb78f1553fa993fd408d224be39ff
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717482"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname

Indique le nom de fichier temporaire du fichier donné au sein du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /gettemporaryname <job> <file_index>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |
| file_index | Démarre à partir de 0. |

## <a name="examples"></a>Exemples

Pour signaler le nom de fichier temporaire du fichier 2 pour le travail nommé *myDownloadJob*:

```
bitsadmin /gettemporaryname myDownloadJob 1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
