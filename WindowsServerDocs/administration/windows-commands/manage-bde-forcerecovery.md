---
title: Manage-bde forcerecovery
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eecae37c-c9a3-46c5-b615-a0ace1f1d778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9b2e37cc57a3aead21f149d157a49587fdcb5f5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724170"
---
# <a name="manage-bde-forcerecovery"></a>Manage-bde : forcerecovery



Force un lecteur protégé par BitLocker en mode de récupération au redémarrage. Cette commande supprime tous les protecteurs de clé associés à Module de plateforme sécurisée (TPM) (TPM) du lecteur. Lorsque l’ordinateur redémarre, seul un mot de passe de récupération ou une clé de récupération peut être utilisé pour déverrouiller le lecteur.

## <a name="syntax"></a>Syntaxe

```
manage-bde –forcerecovery <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
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

Pour illustrer l’utilisation de la commande **-forcerecovery** pour forcer BitLocker à démarrer en mode de récupération sur le lecteur C.
```
manage-bde –forcerecovery C:
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)