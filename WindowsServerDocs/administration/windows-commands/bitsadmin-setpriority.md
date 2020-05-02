---
title: bitsadmin setpriority
description: Rubrique de référence pour la commande Bitsadmin SetPriority, qui définit la priorité du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 556a1d94700780ea22acc1e4c2f32961c0e43342
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717253"
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
| travail | Nom complet ou GUID du travail. |
| priority | Définit la priorité du travail, y compris :<ul><li>FOREGROUND (avant-plan)</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |

## <a name="examples"></a>Exemples

Pour définir la priorité de la tâche nommée *myDownloadJob* sur normal :

```
bitsadmin /setpriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
