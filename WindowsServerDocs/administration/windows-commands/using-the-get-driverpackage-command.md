---
title: À l’aide de la commande get-DriverPackage
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0f123d281625140b3c4ba46316cb9b773bf5fee
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440514"
---
# <a name="using-the-get-driverpackage-command"></a>À l’aide de la commande get-DriverPackage



Affiche des informations sur un package de pilotes sur le serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Paramètres

|        Paramètre         |                                                                           Description                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/ Server :\<nom du serveur >] |              Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.               |
| [/ DriverPackage :\<nom >] |                                                        Spécifie le nom du package de pilotes à afficher.                                                         |
|    [/PackageId:\<ID>]    | Spécifie l’ID de Services de déploiement Windows du package de pilotes s’affiche. Vous devez spécifier l’ID si le package de pilotes ne peut pas être identifié de manière unique par son nom. |
|     [/ Afficher : {pilotes     |                                                                              Fichiers                                                                               |

## <a name="BKMK_examples"></a>Exemples

Pour afficher des informations sur un package de pilotes, tapez une des opérations suivantes :
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)