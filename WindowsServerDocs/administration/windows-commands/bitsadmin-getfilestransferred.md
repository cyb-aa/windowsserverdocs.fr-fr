---
title: bitsadmin getfilestransferred
description: La rubrique commandes Windows pour **Bitsadmin getfilestransferred**, qui récupère le nombre de fichiers transférés pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e282815c-938b-4ac0-a09d-9baafb656dcb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 053b67f5f85066d202b446b31eb1b1698fd735b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850672"
---
# <a name="bitsadmin-getfilestransferred"></a>bitsadmin getfilestransferred

Récupère le nombre de fichiers transférés pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getfilestransferred <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère le nombre de fichiers transférés dans le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getfilestransferred myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)