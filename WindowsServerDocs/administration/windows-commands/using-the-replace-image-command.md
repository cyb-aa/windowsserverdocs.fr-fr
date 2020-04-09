---
title: remplacer-image
description: La rubrique relative aux commandes Windows pour Replace-image, qui remplace une image existante par une nouvelle version de cette image.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68ded3df-e309-420f-9f5d-caeb609385a5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d16ec07171e4560af8074cb9b0be7b6dc252119
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830222"
---
# <a name="using-the-replace-image-command"></a>Utilisation de la commande Replace-image

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remplace une image existante par une nouvelle version de cette image.
## <a name="syntax"></a>Syntaxe
pour les images de démarrage :
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
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média :<Image name>|Spécifie le nom de l’image à remplacer.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : {boot &#124; install}|Spécifie le type d’image à remplacer.|
|/Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image à remplacer. Étant donné qu’il est possible d’avoir le même nom d’image pour différentes images de démarrage dans différentes architectures, la spécification de l’architecture garantit que l’image correcte est remplacée.|
|[/Filename:<File name>]|Si l’image ne peut pas être identifiée de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom du fichier.|
|/replacementImage|Spécifie les paramètres de l’image de remplacement. Vous définissez ces paramètres à l’aide des options suivantes :<p>-mediaFile : <file path> : spécifie le nom et l’emplacement (chemin d’accès complet) du nouveau fichier. wim.<br />-[/SourceImage : <image name>]-spécifie l’image à utiliser si le fichier. wim contient plusieurs images. Cette option s’applique uniquement aux images d’installation.<br />-[/Name :<Image name>] définit le nom d’affichage de l’image.<br />-[/Description :<Image description>]-définit la description de l’image.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour remplacer une image de démarrage, tapez l’une des valeurs suivantes :
```
wdsutil /replace-Imagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /replacementImagmediaFile:C:\MyFolder\Boot.wim
wdsutil /verbose /Progress /replace-Imagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/replacementImagmediaFile:\\MyServer\Share\Boot.wim /Name:My WinPE Image /Description:WinPE Image with drivers
```
Pour remplacer une image d’installation, tapez l’une des options suivantes :
```
wdsutil /replace-Imagmedia:Windows Vista Homemediatype:Install /replacementImagmediaFile:C:\MyFolder\Install.wim
wdsutil /verbose /Progress /replace-Imagmedia:Windows Vista Pro /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:Install.wim /replacementImagmediaFile:\\MyServer\Share \Install.wim /SourceImage:Windows Vista Ultimate /Name:Windows Vista Desktop /Description:Windows Vista Ultimate with standard business applications.
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-image](using-the-add-image-command.md)
[à l’aide de la commande copy-image](using-the-copy-image-command.md)
[à l’aide de la commande Export-image](using-the-export-image-command.md)
[à l’aide de la commande-image](using-the-get-image-command.md)
à l' [aide de la commande Replace-image](using-the-replace-image-command.md)
sous [-commande : Set-image](subcommand-set-image.md)
