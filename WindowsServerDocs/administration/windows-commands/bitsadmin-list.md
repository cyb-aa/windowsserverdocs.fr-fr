---
title: bitsadmin list
description: Rubrique de référence pour la commande de liste Bitsadmin, qui répertorie les tâches de transfert détenues par l’utilisateur actuel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 262589293a147cc1bae98da8fdca047c5f914094
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717425"
---
# <a name="bitsadmin-list"></a>bitsadmin list

Répertorie les tâches de transfert détenues par l’utilisateur actuel.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /list [/allusers][/verbose]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| /ALLUSERS | facultatif. Répertorie les travaux de tous les utilisateurs. Vous devez disposer de privilèges d’administrateur pour utiliser ce paramètre. |
| /verbose | facultatif. Fournit des informations détaillées sur chaque travail. |

## <a name="examples"></a>Exemples

Pour récupérer des informations sur les travaux détenus par l’utilisateur actuel.

```
bitsadmin /list
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
