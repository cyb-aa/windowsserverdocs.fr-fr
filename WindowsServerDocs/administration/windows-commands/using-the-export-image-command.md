---
title: Export-image
description: Rubrique de référence pour Export-image, qui exporte une image existante à partir du magasin d’images vers un autre fichier image Windows (. wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 477859687732fa2cccdc782edb81fdd348f313c5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720914"
---
# <a name="export-image"></a>Export-image

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
Media<Image name>|Spécifie le nom de l’image à exporter.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : {Boot &#124; install}|Spécifie le type d’image à exporter.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images qui contient l’image à exporter. Si aucun nom de groupe d’images n’est spécifié et qu’il n’existe qu’un seul groupe d’images sur le serveur, ce groupe d’images sera utilisé par défaut. Si plusieurs groupes d’images existent sur le serveur, le groupe d’images doit être spécifié.|
|/Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image à exporter. Étant donné qu’il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, la spécification de la valeur d’architecture garantit que l’image correcte sera retournée.|
|[/Filename:<Filename>]|Si l’image ne peut pas être identifiée de manière unique par son nom, le nom de fichier doit être spécifié.|
|/DestinationImage|Spécifie les paramètres de l’image de destination. Vous pouvez spécifier ces paramètres à l’aide des options suivantes :<p>-/FilePath. :<File path and name> -spécifie le chemin d’accès complet du fichier pour la nouvelle image.<br />-[/Name :<Name>]-définit le nom d’affichage de l’image. Si aucun nom n’est spécifié, le nom complet de l’image source est utilisé.<br />-[/Description : <Description>]-Définit la description de l’image.|
|[/Overwrite : {Yes &#124; non &#124; Append}]|Détermine si le fichier spécifié dans l’option **/DestinationImage** sera remplacé si un fichier existant portant ce nom existe déjà dans le/FilePath.<p>-   **Oui** provoque le remplacement du fichier existant.<br />-   **Non** (option par défaut) provoque une erreur si un fichier portant le même nom existe déjà.<br />-   l' **Ajout** entraîne l’ajout de l’image générée sous la forme d’une nouvelle image dans le fichier. wim existant.|
## <a name="examples"></a>Exemples
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
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l’aide[de la commande Add-image](using-the-add-image-command.md)
[à l’aide de la commande Copy-](using-the-copy-image-command.md)
image à l’aide[de](using-the-get-image-command.md)
la commande Set-image à l’aide de la commande[Remove-](using-the-remove-image-command.md)
image à l’aide de la sous-commande de[commande](using-the-replace-image-command.md)
Replace-image[: Set-image](subcommand-set-image.md)
