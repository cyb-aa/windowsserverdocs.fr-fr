---
title: bitsadmin takeownership
description: Rubrique de référence pour la commande Bitsadmin TakeOwnership, qui permet à un utilisateur disposant de privilèges d’administrateur de prendre possession du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5369cb3fa143ebde77ae8cabf04b9a38eed5b9c5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720445"
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
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour prendre possession du travail nommé *myDownloadJob*:

```
bitsadmin /takeownership myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
