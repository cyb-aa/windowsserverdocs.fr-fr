---
title: Convert-RiprepImage
description: Rubrique de référence relative à Convert-RiprepImage, qui convertit une image de préparation de l’installation à distance (RIPrep) existante au format image Windows (. wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12bdd6c49b5fdec0c0e4980a1abf7e21cefc538e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721033"
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
| /FilePath. :\<chemin d’accès et nom du fichier> |                                                                                                                                                                                                       Spécifie le chemin d’accès complet et le nom du fichier. sif qui correspond à l’image RIPrep. Ce fichier est généralement appelé RIPrep. sif et se trouve dans le sous-dossier \Templates du dossier qui contient l’image RIPrep.                                                                                                                                                                                                       |
|        /DestinationImage        | Spécifie les paramètres de l’image de destination, à l’aide des options suivantes.</br>-/FilePath. :\<chemin d’accès et nom du fichier>-définit le chemin d’accès complet au fichier du nouveau fichier. Par exemple : **C:\Temp\convert.wim**</br>-[/Name :\<Name>]-définit le nom d’affichage de l’image. Si aucun nom complet n’est spécifié, le nom complet de l’image source est utilisé.</br>-[/Description : \<Description>]-définit la description de l’image.</br>-[/InPlace] : spécifie que la conversion doit avoir lieu sur l’image RIPrep d’origine et non sur une copie de l’image d’origine, qui est le comportement par défaut.</br>-[/Overwrite : {Oui |

## <a name="examples"></a>Exemples

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