---
title: copier-image
description: Rubrique de référence pour copy-image, qui copie les images qui se trouvent dans le même groupe d’images.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3ffd590682ec36f78d3cbd53fd67fe3b5981e4c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721001"
---
# <a name="copy-image"></a>copier-image

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie les images qui se trouvent dans le même groupe d’images. Pour copier des images entre des groupes d’images, utilisez la commande à l’aide de la commande [Export-image](using-the-export-image-command.md) , puis la commande à l’aide de la commande [Add-image](using-the-add-image-command.md) .

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /copy-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Name:<Name>
         /Filename:<File name>
         [/Description:<Description>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
Media<Image name>|Spécifie le nom de l’image à copier.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : installation|Spécifie le type d’image à copier. Cette option doit être définie sur **installer**.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images qui contient l’image à copier. Si aucun groupe d’images n’est spécifié et qu’un seul groupe existe sur le serveur, ce groupe d’images sera utilisé par défaut. Si plusieurs groupes d'images existent sur le serveur, vous devez spécifier le groupe d'images.|
|[/Filename:<Filename>]|Spécifie le nom de fichier de l’image à copier. Si l’image source ne peut pas être identifiée de manière unique par son nom, vous devez spécifier le nom du fichier.|
|/DestinationImage|Spécifie les paramètres de l’image de destination, comme décrit dans le tableau suivant.<p>-/Name :<Name> -définit le nom complet de l’image à copier.<br />-/Filename :<Filename> -définit le nom du fichier image de destination qui contiendra la copie de l’image.<br />-[/Description : <Description>]-Définit la description de la copie de l’image.|
## <a name="examples"></a>Exemples
Pour créer une copie de l’image spécifiée et la nommer WindowsVista. wim, tapez :
```
wdsutil /copy-Imagmedia:Windows Vista with Officemediatype:Install /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim
```
Pour créer une copie de l’image spécifiée, appliquer les paramètres spécifiés et nommer la copie WindowsVista. wim, tapez :
```
wdsutil /verbose /Progress /copy-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim /Description:This is a copy of the original Windows image with Office installed
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l’aide[de la commande Add-image](using-the-add-image-command.md)
à l’aide de la commande[Export-image](using-the-export-image-command.md)
à l’aide[de la](using-the-get-image-command.md)
commande Set-image à l’aide de la commande[Remove-Image](using-the-remove-image-command.md)
à l’aide de la sous-commande
de[commande Replace-image](using-the-replace-image-command.md)[: Set-image](subcommand-set-image.md)
