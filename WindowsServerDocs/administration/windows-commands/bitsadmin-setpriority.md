---
title: bitsadmin setpriority
description: La rubrique commandes Windows pour **Bitsadmin SetPriority**, qui définit la priorité du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9348680a61649b938267b3277de9aa5aa521361f
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122768"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

Définit la priorité du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setpriority <job> <priority>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| le travail | Nom complet ou GUID du travail. |
| priority | Définit la priorité du travail, y compris :<ul><li>FOREGROUND (avant-plan)</li><li>HIGH (élevée)</li><li>NORMAL (normale)</li><li>LOW (faible)</li></ul> |

## <a name="examples"></a>Exemples

L’exemple suivant définit la priorité de la tâche nommée *myDownloadJob* sur normal.

```
C:\>bitsadmin /setpriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)