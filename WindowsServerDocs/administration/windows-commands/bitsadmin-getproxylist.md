---
title: 'Bitsadmin getproxylist : récupère la liste de proxy pour le travail spécifié.'
description: Rubrique de référence pour la commande Bitsadmin getproxylist, qui récupère la liste de proxy pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a92703d83cc872204d3dc488c15d703dfd50a780
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717654"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Récupère la liste délimitée par des virgules des serveurs proxy à utiliser pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour récupérer la liste de proxy pour le travail nommé *myDownloadJob*:

```
bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
