---
title: bitsadmin info
description: La rubrique commandes Windows pour **Bitsadmin info**, qui affiche des informations récapitulatives sur le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20b8358caba3e0c07b0c985cb24e8f7bde43b06c
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123118"
---
# <a name="bitsadmin-info"></a>bitsadmin info

Affiche des informations récapitulatives sur le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |
| /verbose | Ce paramètre est facultatif. Fournit des informations détaillées sur chaque travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère des informations sur la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)