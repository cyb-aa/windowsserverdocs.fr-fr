---
title: Utilisation de la commande Add-DriverPackage
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f5370d301f5fec15f4812b3d65588297d179455d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363751"
---
# <a name="using-the-add-driverpackage-command"></a>Utilisation de la commande Add-DriverPackage



Ajoute un package de pilotes au serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

## <a name="parameters"></a>Paramètres

|          Paramètre           |                                                              Description                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|   InfFile :\<chemin d’accès du fichier INF >   |                                           Spécifie le chemin d’accès complet du fichier. inf à ajouter.                                            |
|    /Server : nom du serveur\<>    | Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé. |
|      /Architecture : {x86      |                                                                 ia64                                                                  |
| [/DriverGroup : nom de groupe\<>] |                             Spécifie le nom du groupe de pilotes auquel le package doit être ajouté.                              |
|   [/Name :\<nom convivial >]   |                                           Indique le nom convivial du package de pilotes.                                            |

## <a name="BKMK_examples"></a>Illustre

Pour ajouter un package de pilotes, tapez l’un des éléments suivants :
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:"C:\Temp\Display.inf"
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:"C:\Temp\Display.inf" /Architecture:x86 /DriverGroup:x86Drivers /Name:"Display Driver"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

