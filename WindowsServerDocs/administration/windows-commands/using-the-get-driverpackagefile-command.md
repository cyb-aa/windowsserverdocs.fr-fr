---
title: DriverPackageFile
description: La rubrique commandes Windows pour DriverPackageFile, qui affiche des informations sur un package de pilotes, y compris les pilotes et les fichiers qu’il contient.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d485a24479aa857270968a1bff7bd55a014347a3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831032"
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
| /InfFile :\<chemin d’accès du fichier INF > | Spécifie le chemin d’accès complet et le nom de fichier du fichier. inf du package de pilotes. |
|    [/Architecture : {x86    |                                  ia64                                  |
|     [/Show : {drivers      |                                 Files                                  |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour afficher des informations sur un fichier de pilote, tapez :
```
WDSUTIL /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)