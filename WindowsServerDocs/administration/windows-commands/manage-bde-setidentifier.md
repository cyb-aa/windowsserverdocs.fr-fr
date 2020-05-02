---
title: Manage-bde SetIdentifier
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dec19003f9a3421cfd2c73ba892f68aebfb8e133
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724095"
---
# <a name="manage-bde-setidentifier"></a>Manage-bde : SetIdentifier



Définit le champ d’identificateur de lecteur sur le lecteur à la valeur spécifiée dans le paramètre **fournir les identificateurs uniques pour votre organisation** stratégie de groupe.

## <a name="syntax"></a>Syntaxe

```
manage-bde –setidentifier <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
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

Pour illustrer l’utilisation de la commande **-SetIdentifier** pour définir le champ d’identificateur de lecteur BitLocker pour C.
```
manage-bde –setidentifier C:
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)
-   [Utilisation d’agents de récupération de données avec BitLocker](https://technet.microsoft.com/library/dd875560(WS.10).aspx)