---
title: DriverPackageFile
description: Rubrique de référence pour la commande DriverPackageFile, qui affiche des informations sur un package de pilotes, y compris les pilotes et les fichiers qu’il contient.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 71fc38e31471a1deb9d6be29b04d3cd911be1bd6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719931"
---
# <a name="get-driverpackagefile"></a>DriverPackageFile

Affiche des informations sur un package de pilotes, y compris les pilotes et les fichiers qu’il contient.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>Paramètres

|         Paramètre         |                              Description                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile :\<chemin d’accès du fichier INF> | Spécifie le chemin d’accès complet et le nom de fichier du fichier. inf du package de pilotes. |
|    [/Architecture : {x86    |                                  ia64                                  |
|     [/Show : {drivers      |                                 Fichiers                                  |

## <a name="examples"></a>Exemples

Pour afficher des informations sur un fichier de pilote, tapez :
```
WDSUTIL /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)