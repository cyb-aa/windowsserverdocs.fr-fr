---
title: Manage-bde SetIdentifier
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e24719e120430cbfd5192e1800f59f0676aef5cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839872"
---
# <a name="manage-bde-setidentifier"></a>Manage-bde : SetIdentifier



Définit le champ d’identificateur de lecteur sur le lecteur à la valeur spécifiée dans le paramètre **fournir les identificateurs uniques pour votre organisation** stratégie de groupe. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde –setidentifier <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Lecteur \<>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-ComputerName|Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.|
|Nom de l' \<>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche une brève aide à l’invite de commandes.|
|-Help ou-h|Affiche l’aide complète à l’invite de commandes.|

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **-SetIdentifier** pour définir le champ d’identificateur de lecteur BitLocker pour C.
```
manage-bde –setidentifier C:
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)
-   [Utilisation d’agents de récupération de données avec BitLocker](https://technet.microsoft.com/library/dd875560(WS.10).aspx)