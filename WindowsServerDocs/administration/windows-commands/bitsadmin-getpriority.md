---
title: bitsadmin getpriority
description: Rubrique de référence pour la commande Bitsadmin getPriority,, qui récupère la priorité du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 38f92e83ccf5b048d168ce6a21c6026f490b18bf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717677"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin getpriority

Récupère la priorité du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

#### <a name="output"></a>Output

La priorité renvoyée pour cette commande peut être :

- **SOMBRE**

- **HIGH**

- **NORMAL**

- **LOW**

- **CONNUE**

## <a name="examples"></a>Exemples

Pour récupérer la priorité pour la tâche nommée *myDownloadJob*:

```
bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
