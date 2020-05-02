---
title: Manage-bde WipeFreeSpace
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8f1e0f99c226edae467ecb09222b18098ac399ee
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724047"
---
# <a name="manage-bde-wipefreespace"></a>Manage-bde : WipeFreeSpace



Nettoie l’espace libre sur le volume en supprimant tous les fragments de données qui ont pu exister dans l’espace. L’exécution de cette commande sur un volume qui a été chiffré à l’aide de la méthode de chiffrement de l’espace utilisé uniquement offre le même niveau de protection que la méthode de chiffrement du chiffrement de volume complet.

## <a name="syntax"></a>Syntaxe

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Lecteur>|Représente une lettre de lecteur suivie d’un signe deux-points, d’un chemin d’accès de GUID de volume ou d’un volume monté.|
|-Annuler|Annule une réinitialisation de l’espace libre en cours.|
|-ComputerName|Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.|
|\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche une brève aide à l’invite de commandes.|
|-Help ou-h|Affiche l’aide complète à l’invite de commandes.|

## <a name="examples"></a>Exemples

Pour illustrer l’utilisation de la commande **-w** pour créer un nettoyage de l’espace libre sur le lecteur C.
```
manage-bde -w C:
```
Pour illustrer l’utilisation de la commande **-w** avec le paramètre **-Cancel** pour annuler l’effacement de l’espace libre sur le lecteur C.
```
manage-bde -w -Cancel C:
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)