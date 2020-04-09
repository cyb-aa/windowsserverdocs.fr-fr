---
title: bitsadmin getbytestotal
description: La rubrique commandes Windows pour **Bitsadmin getbytestotal**, qui récupère la taille du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84e3e0311ead0eb79f9247d4f06844ece5f20fa2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850782"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

Récupère la taille du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère la taille du travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)