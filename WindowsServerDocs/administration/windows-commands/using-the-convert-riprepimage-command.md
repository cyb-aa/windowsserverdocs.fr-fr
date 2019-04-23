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
ms.openlocfilehash: cf5fffdedbc25ad97e9e96a84d3ff1bbdaf87b2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835050"
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

|Paramètre|Description|
|---------|-----------|
|/ FilePath :\<nom et chemin d’accès du fichier >|Spécifie le chemin d’accès et le nom complet du fichier .sif qui correspond à l’image RIPrep. Ce fichier est généralement appelé Riprep.sif et se trouve dans le sous-dossier \Templates du dossier qui contient l’image RIPrep.|
|/DestinationImage|Spécifie les paramètres de l’image de destination, à l’aide des options suivantes.</br>-FilePath :\<nom et chemin d’accès du fichier >-définit le chemin d’accès de fichier complet pour le nouveau fichier. Exemple : **C:\Temp\convert.wim**</br>-[/ Nom :\<nom >]-définit le nom complet de l’image. Si aucun nom d’affichage n’est spécifié, le nom complet de l’image source sera utilisé.</br>-[/ Description : \<Description >]-définit la description de l’image.</br>-[/InPlace] - Spécifie que la conversion doit prendre place sur l’image RIPrep d’origine et non sur une copie de l’image d’origine, qui est le comportement par défaut.</br>-[/Overwrite : {Oui | Non | Append}] : détermine si le fichier spécifié dans le **/DestinationImage /** option doit être remplacée si un fichier existant portant le même nom existe déjà à FilePath. **Oui** écrase le fichier existant. **Ne** (valeur par défaut) provoque une erreur se produit si un autre fichier portant le même nom existe déjà. **Ajouter** joint de l’image générée sous la forme d’une nouvelle image dans le fichier .wim existant.|

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

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)