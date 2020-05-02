---
title: gérer-mise à niveau de BDE
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23bfa824-6ff0-44cc-9b8b-b199a769fb8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ce7d1805a98a3029ce9b83a1b475ecd8bdb0a4ab
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724055"
---
# <a name="manage-bde-upgrade"></a>Manage-bde : mettre à niveau



Met à niveau la version de BitLocker.

## <a name="syntax"></a>Syntaxe

```
manage-bde -upgrade [<Drive>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
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

Pour illustrer l’utilisation de la commande **-Upgrade** pour mettre à niveau le chiffrement BitLocker sur le lecteur C.
```
manage-bde –upgrade C:
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)
-   [Mise à niveau d’un ordinateur protégé par BitLocker de Windows Vista vers Windows 7](https://technet.microsoft.com/library/ee424325(v=ws.10).aspx)