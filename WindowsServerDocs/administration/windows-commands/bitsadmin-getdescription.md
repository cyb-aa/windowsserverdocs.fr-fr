---
title: bitsadmin getdescription
description: Rubrique relative aux commandes Windows pour **Bitsadmin GetDescription**, qui récupère la description du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ff1638cf634d76001042691fd890dfe41f9ae0b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850722"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription

Récupère la description du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getdescription <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère la description de la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getdescription myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)