---
title: gérer-bde changekey
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 69463db9-7e03-47ff-b233-a95d5055725f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 991d05cb79e6596c3efb3e69b681203838cd8c20
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842090"
---
# <a name="manage-bde-changekey"></a>gérer-bde : changekey



Modifie la clé de démarrage pour un lecteur de système d’exploitation. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde -changekey [<Drive>] [<PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Drive>|Représente une lettre de lecteur suivie par un signe deux-points.|
|\<PathToExternalKeyDirectory>|Représente l’emplacement du répertoire pour enregistrer le fichier de clé de démarrage externe qui peut être utilisé pour déverrouiller le lecteur.|
|-computername|Spécifie que gérer-bde.exe permet de modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **- cn** comme une version abrégée de cette commande.|
|\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche un résumé aide à l’invite de commandes.|
|-help ou-h|Affiche une aide complète à l’invite de commandes.|

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant illustre l’utilisation de la **- changekey** commande pour créer une nouvelle clé de démarrage sur le lecteur E à utiliser avec le chiffrement BitLocker sur le lecteur C.
```
manage-bde –changekey C: E:\
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)