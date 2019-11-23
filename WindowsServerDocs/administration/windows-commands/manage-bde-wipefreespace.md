---
title: Manage-bde WipeFreeSpace
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35d5f5fe079485d35fa412502bec745a136fbce9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373776"
---
# <a name="manage-bde-wipefreespace"></a>Manage-bde : WipeFreeSpace



Nettoie l’espace libre sur le volume en supprimant tous les fragments de données qui ont pu exister dans l’espace. L’exécution de cette commande sur un volume qui a été chiffré à l’aide de la méthode de chiffrement « Space utilisé uniquement » offre le même niveau de protection que la méthode de chiffrement « chiffrement de volume complet ». Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Lecteur \<>|Représente une lettre de lecteur suivie d’un signe deux-points, d’un chemin d’accès de GUID de volume ou d’un volume monté.|
|-Annuler|Annule une réinitialisation de l’espace libre en cours.|
|-ComputerName|Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.|
|\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche une brève aide à l’invite de commandes.|
|-Help ou-h|Affiche l’aide complète à l’invite de commandes.|

## <a name="BKMK_Examples"></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **-w** pour créer un nettoyage de l’espace libre sur le lecteur C.
```
manage-bde -w C:
```
L’exemple suivant illustre l’utilisation de la commande **-w** avec le paramètre **-Cancel** pour annuler l’effacement de l’espace libre sur le lecteur C.
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)