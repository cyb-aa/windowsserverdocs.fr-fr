---
title: bitsadmin takeownership
description: La rubrique commandes Windows pour **Bitsadmin TakeOwnership**, qui permet à un utilisateur disposant de privilèges d’administrateur de prendre possession du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a04f54747e3e06aa61166c2c9f9cedfdfbc8d42a
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122695"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

Permet à un utilisateur disposant de privilèges d’administrateur d’assumer la propriété du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /takeownership <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ---------- |
| Tâche | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

L’exemple suivant prend la propriété de la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /takeownership myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)