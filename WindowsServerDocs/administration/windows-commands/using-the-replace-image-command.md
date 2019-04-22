---
title: À l’aide de la commande replace-Image
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68ded3df-e309-420f-9f5d-caeb609385a5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e396ee678e22885a50c02800d77ecea1cc5ef8ba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819650"
---
# <a name="using-the-replace-image-command"></a>À l’aide de la commande replace-Image

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

remplace une image existante par une nouvelle version de cette image.
## <a name="syntax"></a>Syntaxe
images de démarrage :
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/Name:<Image name>]
         [/Description:<Image description>]
```
pour les images d’installation :
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/SourceImage:<Source image name>]
         [/Name:<Image name>]
         [/Description:<Image description>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média :<Image name>|Spécifie le nom de l’image à remplacer.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
mediatype:{Boot &#124; Install}|Spécifie le type d’image à remplacer.|
|/ Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image à remplacer. Comme il est possible d’avoir le même nom d’image pour les images de démarrage différents dans différentes architectures, en spécifiant l’architecture permet de s’assurer que l’image correcte est remplacé.|
|[/Filename:<File name>]|Si l’image ne peut pas être identifié de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom de fichier.|
|/replacementImage|Spécifie les paramètres de l’image de remplacement. Vous définissez ces paramètres à l’aide des options suivantes :<br /><br />-mediaFile : <file path> -Spécifie le nom et l’emplacement (chemin d’accès complet) du nouveau fichier .wim.<br />-[/ SourceImage : <image name>]-Spécifie l’image à utiliser si le fichier .wim contient plusieurs images. Cette option s’applique uniquement aux images d’installation.<br />-[/ Nom :<Image name>] définit le nom complet de l’image.<br />-[/ Description :<Image description>]-définit la description de l’image.|
## <a name="BKMK_examples"></a>Exemples
Pour remplacer une image de démarrage, tapez une des opérations suivantes :
```
wdsutil /replace-Imagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /replacementImagmediaFile:"C:\MyFolder\Boot.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/replacementImagmediaFile:\\MyServer\Share\Boot.wim /Name:"My WinPE Image" /Description:"WinPE Image with drivers"
```
Pour remplacer une image d’installation, tapez une des opérations suivantes :
```
wdsutil /replace-Imagmedia:"Windows Vista Homemediatype:Install /replacementImagmediaFile:"C:\MyFolder\Install.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"Windows Vista Pro" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:Install.wim /replacementImagmediaFile:\\MyServer\Share \Install.wim /SourceImage:"Windows Vista Ultimate" /Name:"Windows Vista Desktop" /Description:"Windows Vista Ultimate with standard business applications."
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-Image](using-the-add-image-command.md)
[à l’aide de la commande de copie-Image](using-the-copy-image-command.md)
[à l’aide du Commande export-Image](using-the-export-image-command.md)
[à l’aide de la commande get-Image](using-the-get-image-command.md)
[à l’aide de l’Image de remplacement commande](using-the-replace-image-command.md) 
 [ Sous-commande : set-Image](subcommand-set-image.md)
