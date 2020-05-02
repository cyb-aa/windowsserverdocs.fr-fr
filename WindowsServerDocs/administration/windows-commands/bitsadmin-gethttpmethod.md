---
title: bitsadmin gethttpmethod
description: Rubrique de référence pour la commande Bitsadmin gethttpmethod, qui obtient le verbe HTTP à utiliser avec le travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a458322a5ace69df74df054a537a7365da9e7329
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717885"
---
# <a name="bitsadmin-gethttpmethod"></a>bitsadmin gethttpmethod

Obtient le verbe HTTP à utiliser avec le travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /gethttpmethod <Job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour récupérer le verbe HTTP à utiliser avec la tâche nommée *myDownloadJob*:

```
bitsadmin /gethttpmethod myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
