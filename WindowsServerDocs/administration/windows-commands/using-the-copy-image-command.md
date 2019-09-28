---
title: Utilisation de la commande copy-image
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f269884e9c1f96151c9e1220242fad917b76831
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363559"
---
# <a name="using-the-copy-image-command"></a>Utilisation de la commande copy-image

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Copie les images qui se trouvent dans le même groupe d’images. Pour copier des images entre des groupes d’images, utilisez la commande à l’aide de la commande [Export-image](using-the-export-image-command.md) , puis la commande à l’aide de la commande [Add-image](using-the-add-image-command.md) .
pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).
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
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média : <Image name>|Spécifie le nom de l’image à copier.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : installation|Spécifie le type d’image à copier. Cette option doit être définie sur **installer**.|
|\mediaGroup : <Image group name>]|Spécifie le groupe d’images qui contient l’image à copier. Si aucun groupe d’images n’est spécifié et qu’un seul groupe existe sur le serveur, ce groupe d’images sera utilisé par défaut. Si plusieurs groupes d'images existent sur le serveur, vous devez spécifier le groupe d'images.|
|[/Filename:<Filename>]|Spécifie le nom de fichier de l’image à copier. Si l’image source ne peut pas être identifiée de manière unique par son nom, vous devez spécifier le nom du fichier.|
|/DestinationImage|Spécifie les paramètres de l’image de destination, comme décrit dans le tableau suivant.<br /><br />-/Name : <Name>-définit le nom complet de l’image à copier.<br />-/Filename : <Filename>-définit le nom du fichier image de destination qui contiendra la copie de l’image.<br />-[/Description : <Description>]-Définit la description de la copie de l’image.|
## <a name="BKMK_examples"></a>Illustre
Pour créer une copie de l’image spécifiée et la nommer WindowsVista. wim, tapez :
```
wdsutil /copy-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim"
```
Pour créer une copie de l’image spécifiée, appliquer les paramètres spécifiés et nommer la copie WindowsVista. wim, tapez :
```
wdsutil /verbose /Progress /copy-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim" /Description:"This is a copy of the original Windows image with Office installed"
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-image](using-the-add-image-command.md)
[à l’aide de la commande Export-image](using-the-export-image-command.md)
[à l’aide de la commande-image](using-the-get-image-command.md)
 à l’aide de[la commande Remove-Image](using-the-remove-image-command.md)
[à l’aide de l’option Commande Replace-image](using-the-replace-image-command.md)1 sous-[commande : Set-image](subcommand-set-image.md)
