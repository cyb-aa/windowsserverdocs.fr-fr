---
title: 'Bitsadmin getproxylist : récupère la liste de proxy pour le travail spécifié.'
description: La rubrique commandes Windows pour **Bitsadmin getproxylist**, qui récupère la liste de proxy pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c0d26fb074bd1b792caa7fe2ce8fd31b64365e2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850522"
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
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère la liste de proxy pour la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)