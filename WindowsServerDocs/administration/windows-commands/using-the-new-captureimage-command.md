---
title: À l’aide de la commande Nouveau CaptureImage
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 00f15ae7ee1a7ab1ac1f71599d2cae9bb51d921e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440414"
---
# <a name="using-the-new-captureimage-command"></a>À l’aide de la commande Nouveau CaptureImage



Crée une nouvelle image de capture à partir d’une image de démarrage. Images de capture sont utilitaire au lieu de démarrer le programme d’installation de capture des images de démarrage que vous démarrez les Services de déploiement Windows. Lorsque vous démarrez un ordinateur de référence (qui a été préparé avec Sysprep) dans une image de capture, un Assistant crée une image d’installation de l’ordinateur de référence et l’enregistre sous la forme d’un fichier Image Windows (.wim). Vous pouvez également ajouter l’image sur un support (par exemple, un lecteur de CD, DVD ou USB) et puis démarrez un ordinateur à partir de ce support. Après avoir créé l’image d’installation, vous pouvez ajouter l’image sur le serveur pour le déploiement de démarrage PXE. Pour plus d’informations, consultez Création d’Images ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

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
| [/ Server :\<nom du serveur >] |                                                                                                                                       Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.                                                                                                                                        |
|   / Image :\<nom de l’Image >   |                                                                                                                                                                                                         Spécifie le nom de l’image de démarrage source.                                                                                                                                                                                                         |
|   / Architecture : {x 86    |                                                                                                                                                                                                                             ia64                                                                                                                                                                                                                             |
| [/ Nom de fichier : \<Filename>] |                                                                                                                                                                            Si l’image ne peut pas être identifié de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom de fichier.                                                                                                                                                                            |
|    /DestinationImage     | Spécifie les paramètres de l’image de destination. Vous spécifiez les paramètres à l’aide des options suivantes :</br>-   /FilePath: \<Nom et chemin d’accès du fichier > définit le chemin d’accès de fichier complet de la nouvelle image de capture.</br>-[/ Nom : \<Nom >]-définit le nom complet de l’image. Si aucun nom d’affichage n’est spécifié, le nom complet de l’image source sera utilisé.</br>-[/ Description : \<Description >]-définit la description de l’image.</br>-[/Overwrite : {Oui |

## <a name="BKMK_examples"></a>Exemples

Pour créer une image de capture et nommez-le WinPECapture.wim, tapez :
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