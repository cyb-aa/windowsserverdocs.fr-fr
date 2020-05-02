---
title: Manage-bde changepin
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c85aa1c7-3485-4839-a292-99dfcd6db252
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbf53372488db76689be9d2a4d513553f4ad0de3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724187"
---
# <a name="manage-bde-changepin"></a>Manage-bde : changepin



Modifie le code confidentiel d’un lecteur de système d’exploitation. L’utilisateur est invité à entrer un nouveau code confidentiel.

## <a name="syntax"></a>Syntaxe

```
manage-bde -changepin [<Drive>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Lecteur>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-ComputerName|Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.|
|\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche une brève aide à l’invite de commandes.|
|-Help ou-h|Affiche l’aide complète à l’invite de commandes.|

## <a name="examples"></a>Exemples

Pour illustrer l’utilisation de la commande **-changepin** pour modifier le code confidentiel utilisé avec BitLocker sur le lecteur C.
```
manage-bde –changepin C:
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)