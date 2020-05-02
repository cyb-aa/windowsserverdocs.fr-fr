---
title: bitsadmin getminretrydelay
description: Rubrique de référence pour la commande Bitsadmin getminretrydelay, qui récupère la durée, en secondes, pendant laquelle le service attend après avoir rencontré une erreur temporaire avant de tenter de transférer le fichier.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a50b9d98fe0b873dc58b8e86dc672a8f4157208a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717848"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay

Récupère la durée, en secondes, pendant laquelle le service attendra après avoir rencontré une erreur temporaire avant de tenter de transférer le fichier.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getminretrydelay <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour récupérer le délai minimal entre deux tentatives pour la tâche nommée *myDownloadJob*:

```
bitsadmin /getminretrydelay myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
