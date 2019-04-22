---
title: À l’aide de la commande Export-Image
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: efa9b2d09c37a383a91883ee02c995eedb2f235e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823140"
---
# <a name="using-the-export-image-command"></a>À l’aide de la commande Export-Image

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exporte une image existante du magasin d’images vers un autre fichier Image Windows (.wim).
## <a name="syntax"></a>Syntaxe
images de démarrage :
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
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média :<Image name>|Spécifie le nom de l’image à exporter.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
mediatype:{Boot &#124; Install}|Spécifie le type d’image à exporter.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images contenant l’image à exporter. Si aucun nom de groupe d’image n’est spécifié et qu’une seule image existe sur le serveur, ce groupe d’images est utilisé par défaut. Si plus d’un groupe d’images existe sur le serveur, vous devez spécifier le groupe d’images.|
|/ Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image à exporter. Comme il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, en spécifiant la valeur de l’architecture permet de s’assurer que l’image correcte sera retourné.|
|[/Filename:<Filename>]|Si l’image ne peut pas être identifié de manière unique par son nom, le nom de fichier doit être spécifié.|
|/DestinationImage|Spécifie les paramètres de l’image de destination. Vous pouvez spécifier ces paramètres à l’aide des options suivantes :<br /><br />-FilePath :<File path and name> -Spécifie le chemin d’accès de fichier complet de la nouvelle image.<br />-[/ Nom :<Name>]-définit le nom complet de l’image. Si aucun nom n’est spécifié, le nom complet de l’image source sera utilisé.<br />-[/ Description : <Description>]-Définit la description de l’image.|
|[/Overwrite : {Oui &#124; non &#124; append}]|Détermine si le fichier spécifié dans le **/DestinationImage /** option sera remplacée s’il existe déjà un fichier existant portant le même nom dans /FilePath.<br /><br />-   **Oui** entraîne le remplacement du fichier existant.<br />-   **Ne** (l’option par défaut) provoque une erreur se produit si un fichier portant le même nom existe déjà.<br />-   **ajouter** provoque l’image générée à ajouter en tant qu’une nouvelle image dans le fichier .wim existant.|
## <a name="BKMK_examples"></a>Exemples
Pour exporter une image de démarrage, tapez une des opérations suivantes :
```
wdsutil /Export-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /DestinationImage /Filepath:"C:\temp\boot.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/DestinationImage /Filepath:"\\Server\Share\ExportImage.wim" /Name:"Exported WinPE image" /Description:"WinPE Image from WDS server" /Overwrite:Yes
```
Pour exporter une image d’installation, tapez une des opérations suivantes :
```
wdsutil /Export-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Filepath:"C:\Temp\Install.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:"Exported Windows image" /Description:"Windows Vista image from WDS server" /Overwrite:append
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-Image](using-the-add-image-command.md)
[à l’aide de la commande de copie-Image](using-the-copy-image-command.md)
[à l’aide de l’Image de get Commande](using-the-get-image-command.md)
[à l’aide de la commande remove-Image](using-the-remove-image-command.md)
[à l’aide de l’Image de remplacement commande](using-the-replace-image-command.md)
[sous-commande : set-Image](subcommand-set-image.md)
