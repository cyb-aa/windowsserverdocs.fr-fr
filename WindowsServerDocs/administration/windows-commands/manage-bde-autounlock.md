---
title: gérer le déverrouillage autodéverrouillé BDE
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6927c964a3f35c70fc3b9467cc2d16bbd973b17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374141"
---
# <a name="manage-bde-autounlock"></a>Manage-bde : autounlock



Gère le déverrouillage automatique des lecteurs de données protégés par BitLocker. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]

```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-activer|Active le déverrouillage automatique pour un lecteur de données.|
|-désactiver|Désactive le déverrouillage automatique pour un lecteur de données.|
|-clearallkeys|Supprime toutes les clés externes stockées sur le lecteur du système d’exploitation.|
|@no__t 0Drive >|Représente une lettre de lecteur suivie par un signe deux-points.|
|-ComputerName|Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.|
|\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche une brève aide à l’invite de commandes.|
|-Help ou-h|Affiche l’aide complète à l’invite de commandes.|

## <a name="BKMK_Examples"></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **-autounlock** pour activer le déverrouillage automatique du lecteur de données E.
```
manage-bde –autounlock -enable E:
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)