---
title: Utilisation de la commande New-CaptureImage
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb48cb76ef99eac51b862a5e1a3d1999a1cfc89d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363050"
---
# <a name="using-the-new-captureimage-command"></a>Utilisation de la commande New-CaptureImage



crée une image de capture à partir d’une image de démarrage existante. Les images de capture sont des images de démarrage qui démarrent l’utilitaire de capture des services de déploiement Windows au lieu de démarrer le programme d’installation. Lorsque vous démarrez un ordinateur de référence (qui a été préparé avec Sysprep) dans une image de capture, un Assistant crée une image d’installation de l’ordinateur de référence et l’enregistre sous la forme d’un fichier image Windows (. wim). Vous pouvez également ajouter l’image au média (par exemple, un CD, un DVD ou un lecteur USB), puis démarrer un ordinateur à partir de ce média. Après avoir créé l’image d’installation, vous pouvez ajouter l’image au serveur pour le déploiement de démarrage PXE. Pour plus d’informations, consultez Création d’images ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /New-CaptureImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
        /FilePath:<File path and name>
        [/Name:<Name>]
        [/Description:<Description>]
        [/Overwrite:{Yes | No | Append}]
        [/UnattendFilePath:<File path>]
```

## <a name="parameters"></a>Paramètres

|        Paramètre         |                                                                                                                                                                                                                         Description                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server : @no__t-nom 0Server >] |                                                                                                                                       Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                                                        |
|   /Image : nom @no__t 0Image >   |                                                                                                                                                                                                         Spécifie le nom de l’image de démarrage source.                                                                                                                                                                                                         |
|   /Architecture : {x86    |                                                                                                                                                                                                                             ia64                                                                                                                                                                                                                             |
| /Filename @no__t 0Filename >] |                                                                                                                                                                            Si l’image ne peut pas être identifiée de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom du fichier.                                                                                                                                                                            |
|    /DestinationImage     | Spécifie les paramètres de l’image de destination. Vous spécifiez les paramètres à l’aide des options suivantes :</br>/FilePath. le chemin d’accès et le nom de @no__t 0File > définit le chemin de fichier complet de la nouvelle image de capture.</br>-[/Name : \<Name >]-définit le nom d’affichage de l’image. Si aucun nom complet n’est spécifié, le nom complet de l’image source est utilisé.</br>-[/Description : \<Description >]-définit la description de l’image.</br>-[/Overwrite : {Oui |

## <a name="BKMK_examples"></a>Illustre

Pour créer une image de capture et la nommer WinPECapture. wim, tapez :
```
WDSUTIL /New-CaptureImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPECapture.wim"
```
Pour créer une image de capture et appliquer les paramètres spécifiés, tapez :
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim 
/DestinationImage /FilePath:"\\Server\Share\WinPECapture.wim" /Name:"New WinPE image" /Description:"WinPE image with capture utility" /Overwrite:No /UnattendFilePath:"\\Server\Share\WDSCapture.inf"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)