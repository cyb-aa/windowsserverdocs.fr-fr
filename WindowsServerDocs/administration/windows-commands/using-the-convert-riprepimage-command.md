---
title: À l’aide de la commande convert-RiprepImage
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b41b6dcc52c3e6700d1d18c61eceea8b990ecdf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440582"
---
# <a name="using-the-convert-riprepimage-command"></a>À l’aide de la commande convert-RiprepImage



Convertit une image existante de la préparation de l’Installation à distance (RIPrep) au format d’Image Windows (.wim).

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /Convert-RIPrepImage /FilePath:<File path and name>
     /DestinationImage
         /FilePath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
         [/InPlace]
         [/Overwrite:{Yes | No | Append}]
```

## <a name="parameters"></a>Paramètres

|            Paramètre            |                                                                                                                                                                                                                                                                                                               Description                                                                                                                                                                                                                                                                                                                |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| / FilePath :\<nom et chemin d’accès du fichier > |                                                                                                                                                                                                       Spécifie le chemin d’accès et le nom complet du fichier .sif qui correspond à l’image RIPrep. Ce fichier est généralement appelé Riprep.sif et se trouve dans le sous-dossier \Templates du dossier qui contient l’image RIPrep.                                                                                                                                                                                                       |
|        /DestinationImage        | Spécifie les paramètres de l’image de destination, à l’aide des options suivantes.</br>-FilePath :\<nom et chemin d’accès du fichier >-définit le chemin d’accès de fichier complet pour le nouveau fichier. Exemple : **C:\Temp\convert.wim**</br>-[/ Nom :\<nom >]-définit le nom complet de l’image. Si aucun nom d’affichage n’est spécifié, le nom complet de l’image source sera utilisé.</br>-[/ Description : \<Description >]-définit la description de l’image.</br>-[/InPlace] - Spécifie que la conversion doit prendre place sur l’image RIPrep d’origine et non sur une copie de l’image d’origine, qui est le comportement par défaut.</br>-[/Overwrite : {Oui |

## <a name="BKMK_examples"></a>Exemples

Pour convertir l’image RIPrep.sif spécifiée RIPREP.wim, tapez :
```
WDSUTIL /Convert-RiPrepImage /FilePath:"R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif" /DestinationImage
/FilePath:"C:\Temp\RIPREP.wim"
```
Pour convertir l’image RIPrep.sif spécifiée RIPREP.wim avec le nom spécifié et la description et remplacez-le par le nouveau fichier si un fichier existe déjà, tapez :
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:"\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif"
/DestinationImage /FilePath:"\\Server\Share\RIPREP.wim"
/Name:"WindowsXP image" /Description:"Converted RIPREP image of WindowsXP"
/Overwrite:Append
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)