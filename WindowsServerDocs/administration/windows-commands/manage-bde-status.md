---
title: gérer-État BDE
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 235db54ef2361c0e95c66b15a15be7f188fb74d9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373857"
---
# <a name="manage-bde-status"></a>Manage-bde : Status



Fournit les informations suivantes sur tous les lecteurs de l’ordinateur ; qu’ils soient ou non protégés par BitLocker :
-   Size
-   Version de BitLocker
-   État de la conversion
-   Pourcentage chiffré
-   Méthode de chiffrement
-   État de protection
-   État du verrouillage
-   Champ d’identification
-   Protecteurs de clés

Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde -status [<Drive>] [-protectionaserrorlevel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0Drive >|Représente une lettre de lecteur suivie par un signe deux-points.|
|-protectionaserrorlevel|Force l’outil en ligne de commande Manage-bde à envoyer le code de retour 0 lorsque le volume est protégé et 1 lorsque le volume n’est pas protégé. le plus souvent utilisé pour les scripts batch pour déterminer si un lecteur est protégé par BitLocker. Vous pouvez également utiliser **-p** comme version abrégée de cette commande.|
|-ComputerName|Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.|
|\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche une brève aide à l’invite de commandes.|
|-Help ou-h|Affiche l’aide complète à l’invite de commandes.|

## <a name="BKMK_Examples"></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **-Status** pour afficher l’état du lecteur C.
```
manage-bde –status C:
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)