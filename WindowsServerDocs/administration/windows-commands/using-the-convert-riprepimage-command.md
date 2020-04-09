---
title: Convert-RiprepImage
description: La rubrique relative aux commandes Windows pour Convert-RiprepImage, qui convertit une image de préparation de l’installation à distance (RIPrep) existante au format image Windows (. wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01e33580a6d2da55df15fabde70697c22f894f7c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831812"
---
# <a name="convert-riprepimage"></a>Convert-RiprepImage

Convertit une image de préparation de l’installation à distance (RIPrep) existante au format image Windows (. wim).

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

### <a name="parameters"></a>Paramètres

|            Paramètre            |                                                                                                                                                                                                                                                                                                               Description                                                                                                                                                                                                                                                                                                                |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /FilePath. : chemin d’accès et nom du fichier de\<> |                                                                                                                                                                                                       Spécifie le chemin d’accès complet et le nom du fichier. sif qui correspond à l’image RIPrep. Ce fichier est généralement appelé RIPrep. sif et se trouve dans le sous-dossier \Templates du dossier qui contient l’image RIPrep.                                                                                                                                                                                                       |
|        /DestinationImage        | Spécifie les paramètres de l’image de destination, à l’aide des options suivantes.</br>-/FilePath. :\<chemin d’accès et nom de fichier >-définit le chemin de fichier complet du nouveau fichier. Par exemple : **C:\Temp\convert.wim**</br>-[/Name :\<name >]-définit le nom d’affichage de l’image. Si aucun nom complet n’est spécifié, le nom complet de l’image source est utilisé.</br>-[/Description : \<Description >]-définit la description de l’image.</br>-[/InPlace] : spécifie que la conversion doit avoir lieu sur l’image RIPrep d’origine et non sur une copie de l’image d’origine, qui est le comportement par défaut.</br>-[/Overwrite : {Oui |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour convertir l’image RIPrep. sif spécifiée en RIPREP. wim, tapez :
```
WDSUTIL /Convert-RiPrepImage /FilePath:R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif /DestinationImage
/FilePath:C:\Temp\RIPREP.wim
```
Pour convertir l’image RIPrep. sif spécifiée en RIPREP. wim avec le nom et la description spécifiés, et la remplacer par le nouveau fichier si un fichier existe déjà, tapez :
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif
/DestinationImage /FilePath:\\Server\Share\RIPREP.wim
/Name:WindowsXP image /Description:Converted RIPREP image of WindowsXP
/Overwrite:Append
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)