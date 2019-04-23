---
title: À l’aide de la commande de copie-Image
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 52c9c0bb45e60e76077bf90534e93f2c6fc1df18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884040"
---
# <a name="using-the-copy-image-command"></a>À l’aide de la commande de copie-Image

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Images de copies qui se trouvent dans le même groupe d’images. Pour copier des images entre les groupes d’images, utilisez le [à l’aide de la commande Export-Image](using-the-export-image-command.md) commande, puis le [à l’aide de la commande add-Image](using-the-add-image-command.md) commande.
Pour obtenir des exemples de la façon dont vous pouvez utiliser cette commande, consultez [exemples](#BKMK_examples).
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
média :<Image name>|Spécifie le nom de l’image à copier.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
MediaType:Install|Spécifie le type d’image à copier. Cette option doit être définie sur **installer**.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images qui contient l’image à copier. Si aucun groupe d’images n’est spécifié et qu’un seul groupe existe sur le serveur, ce groupe d’images est utilisé par défaut. Si plusieurs groupes d'images existent sur le serveur, vous devez spécifier le groupe d'images.|
|[/Filename:<Filename>]|Spécifie le nom de fichier de l’image à copier. Si l’image source ne peut pas être identifié de manière unique par son nom, vous devez spécifier le nom de fichier.|
|/DestinationImage|Spécifie les paramètres de l’image de destination, comme décrit dans le tableau suivant.<br /><br />-/Name :<Name> -définit le nom complet de l’image à copier.<br />-Argument /Filename :<Filename> -définit le nom du fichier d’image de destination qui contiendra la copie de l’image.<br />-[/ Description : <Description>]-Définit la description de la copie de l’image.|
## <a name="BKMK_examples"></a>Exemples
Pour créer une copie de l’image spécifiée et nommez-le WindowsVista.wim, tapez :
```
wdsutil /copy-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim"
```
Pour créer une copie de l’image spécifiée, s’appliquent les paramètres spécifiés et nommez la copie WindowsVista.wim, type :
```
wdsutil /verbose /Progress /copy-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim" /Description:"This is a copy of the original Windows image with Office installed"
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-Image](using-the-add-image-command.md)
[à l’aide de la commande Export-Image](using-the-export-image-command.md)
[à l’aide du Get-Image commande](using-the-get-image-command.md)
[à l’aide de la commande remove-Image](using-the-remove-image-command.md)
[à l’aide de l’Image de remplacement commande](using-the-replace-image-command.md) 
 [ Sous-commande : set-Image](subcommand-set-image.md)
