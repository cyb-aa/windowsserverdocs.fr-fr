---
title: gérer-État BDE
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c8248333944b030dc8868ba4408024727a08c3a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839842"
---
# <a name="manage-bde-status"></a>Manage-bde : Status



Fournit les informations suivantes sur tous les lecteurs de l’ordinateur ; qu’ils soient ou non protégés par BitLocker :
-   Taille
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

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Lecteur \<>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-protectionaserrorlevel|Force l’outil en ligne de commande Manage-bde à envoyer le code de retour 0 lorsque le volume est protégé et 1 lorsque le volume n’est pas protégé. le plus souvent utilisé pour les scripts batch pour déterminer si un lecteur est protégé par BitLocker. Vous pouvez également utiliser **-p** comme version abrégée de cette commande.|
|-ComputerName|Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande.|
|Nom de l' \<>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche une brève aide à l’invite de commandes.|
|-Help ou-h|Affiche l’aide complète à l’invite de commandes.|

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **-Status** pour afficher l’état du lecteur C.
```
manage-bde –status C:
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Gérer-bde](manage-bde.md)