---
title: Add-DriverPackage
description: Rubrique de référence pour Add-DriverPackage, qui ajoute un package de pilotes au serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12d7f7078cf3dde10f834a4d4c7784ecc1d9bdf2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721094"
---
# <a name="add-driverpackage"></a>Add-DriverPackage

Ajoute un package de pilotes au serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

### <a name="parameters"></a>Paramètres

|          Paramètre           |                                                              Description                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|   InfFile :\<chemin d’accès du fichier INF>   |                                           Spécifie le chemin d’accès complet du fichier. inf à ajouter.                                            |
|    /Server :\<nom du serveur>    | Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé. |
|      /Architecture : {x86      |                                                                 ia64                                                                  |
| [/DriverGroup :\<nom de groupe>] |                             Spécifie le nom du groupe de pilotes auquel le package doit être ajouté.                              |
|   [/Name :\<nom convivial>]   |                                           Indique le nom convivial du package de pilotes.                                            |

## <a name="examples"></a>Exemples

Pour ajouter un package de pilotes, tapez l’un des éléments suivants :
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:C:\Temp\Display.inf
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:C:\Temp\Display.inf /Architecture:x86 /DriverGroup:x86Drivers /Name:Display Driver
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

