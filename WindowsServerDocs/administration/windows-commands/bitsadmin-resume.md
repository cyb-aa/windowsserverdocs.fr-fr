---
title: bitsadmin resume
description: La rubrique commandes Windows pour **Bitsadmin Resume**, qui active un travail nouveau ou suspendu dans la file d’attente de transfert.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e81bd80232cd4ec8fbba70c86cd97bb9695680f8
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123078"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

Active une tâche nouvelle ou suspendue dans la file d’attente de transfert.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

L’exemple suivant reprend la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)