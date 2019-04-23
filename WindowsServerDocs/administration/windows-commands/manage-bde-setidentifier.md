---
title: gérer-bde setidentifier
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e75483985624e77c5ea454bc3de299c6d0c31035
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831100"
---
# <a name="manage-bde-setidentifier"></a>gérer-bde : setidentifier



Définit le champ d’identificateur de lecteur sur le lecteur à la valeur spécifiée dans le **fournir les identificateurs uniques pour votre organisation** paramètre de stratégie de groupe. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
manage-bde –setidentifier <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Drive>|Représente une lettre de lecteur suivie par un signe deux-points.|
|-computername|Spécifie que gérer-bde.exe permet de modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **- cn** comme une version abrégée de cette commande.|
|\<Name>|Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.|
|-? ou /?|Affiche un résumé aide à l’invite de commandes.|
|-help ou-h|Affiche une aide complète à l’invite de commandes.|

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant illustre l’utilisation de la **- setidentifier** commande pour définir le champ d’identificateur de lecteur BitLocker pour C.
```
manage-bde –setidentifier C:
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)
-   [À l’aide des Agents de récupération de données avec BitLocker](https://technet.microsoft.com/library/dd875560(WS.10).aspx)