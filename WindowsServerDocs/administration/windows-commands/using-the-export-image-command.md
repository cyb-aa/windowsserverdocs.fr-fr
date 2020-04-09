---
title: Export-image
description: Rubrique relative aux commandes Windows pour Export-image, qui exporte une image existante à partir du magasin d’images vers un autre fichier image Windows (. wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3e45c254cc6782a61828fa12e479110836e5de5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831422"
---
# <a name="export-image"></a>Export-image

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exporte une image existante à partir du magasin d’images vers un autre fichier image Windows (. wim).

## <a name="syntax"></a>Syntaxe
pour les images de démarrage :
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No}]
```
pour les images d’installation :
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No | append}]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média :<Image name>|Spécifie le nom de l’image à exporter.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : {boot &#124; install}|Spécifie le type d’image à exporter.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images qui contient l’image à exporter. Si aucun nom de groupe d’images n’est spécifié et qu’il n’existe qu’un seul groupe d’images sur le serveur, ce groupe d’images sera utilisé par défaut. Si plusieurs groupes d’images existent sur le serveur, le groupe d’images doit être spécifié.|
|/Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image à exporter. Étant donné qu’il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, la spécification de la valeur d’architecture garantit que l’image correcte sera retournée.|
|[/Filename:<Filename>]|Si l’image ne peut pas être identifiée de manière unique par son nom, le nom de fichier doit être spécifié.|
|/DestinationImage|Spécifie les paramètres de l’image de destination. Vous pouvez spécifier ces paramètres à l’aide des options suivantes :<p>-/FilePath. :<File path and name> : spécifie le chemin d’accès complet du fichier pour la nouvelle image.<br />-[/Name :<Name>]-définit le nom d’affichage de l’image. Si aucun nom n’est spécifié, le nom complet de l’image source est utilisé.<br />-[/Description : <Description>]-Définit la description de l’image.|
|[/Overwrite : {Yes &#124; not &#124; Append}]|Détermine si le fichier spécifié dans l’option **/DestinationImage** sera remplacé si un fichier existant portant ce nom existe déjà dans le/FilePath.<p>-   **Oui** provoque le remplacement du fichier existant.<br />-   **non** (option par défaut) provoque l’apparition d’une erreur si un fichier portant le même nom existe déjà.<br />-   **Append** entraîne l’ajout de l’image générée sous la forme d’une nouvelle image dans le fichier. wim existant.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour exporter une image de démarrage, tapez l’une des options suivantes :
```
wdsutil /Export-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86 /DestinationImage /Filepath:C:\temp\boot.wim
wdsutil /verbose /Progress /Export-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/DestinationImage /Filepath:\\Server\Share\ExportImage.wim /Name:Exported WinPE image /Description:WinPE Image from WDS server /Overwrite:Yes
```
Pour exporter une image d’installation, tapez l’une des options suivantes :
```
wdsutil /Export-Imagmedia:Windows Vista with Officemediatype:Install /DestinationImage /Filepath:C:\Temp\Install.wim
wdsutil /verbose /Progress /Export-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:Exported Windows image /Description:Windows Vista image from WDS server /Overwrite:append
```
## <a name="additional-references"></a>Références supplémentaires
- La [clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-image](using-the-add-image-command.md)
[à l’aide de la commande copy-image](using-the-copy-image-command.md)
[à l’aide de la commande](using-the-get-image-command.md) de l’image-image
à l’aide [de la commande Remove-Image](using-the-remove-image-command.md)
[à l’aide de la commande Replace-image](using-the-replace-image-command.md)
sous- [commande : Set-image](subcommand-set-image.md)
