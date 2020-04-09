---
title: cache Bitsadmin et setLimit
description: Rubrique relative aux commandes Windows pour le **cache Bitsadmin et setLimit**, qui définit la limite de taille du cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 746ee0b69da8f5bd22fec2ccbd432126cc25d94d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850872"
---
# <a name="bitsadmin-cache-and-setlimit"></a>cache Bitsadmin et setLimit

Définit la limite de taille du cache.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /cache /setlimit percent
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| percent | Limite du cache définie sous la forme d’un pourcentage de l’espace total du disque dur. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant limite la taille du cache à 50%.

```
C:\>bitsadmin /cache /setlimit 50
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)