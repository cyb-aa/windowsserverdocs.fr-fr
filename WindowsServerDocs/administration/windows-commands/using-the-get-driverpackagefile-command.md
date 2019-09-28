---
title: Utilisation de la commande DriverPackageFile
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21bbe17e56177da5cd2c1bf83c712d256cc794c8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363148"
---
# <a name="using-the-get-driverpackagefile-command"></a>Utilisation de la commande DriverPackageFile



Affiche des informations sur un package de pilotes, y compris les pilotes et les fichiers qu’il contient.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Paramètres

|         Paramètre         |                              Description                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile : chemin d’accès du fichier \<Inf > | Spécifie le chemin d’accès complet et le nom de fichier du fichier. inf du package de pilotes. |
|    [/Architecture : {x86    |                                  ia64                                  |
|     [/Show : {drivers      |                                 Fichiers                                  |

## <a name="BKMK_examples"></a>Illustre

Pour afficher des informations sur un fichier de pilote, tapez :
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)