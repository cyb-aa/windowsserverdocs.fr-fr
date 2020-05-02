---
title: Ajouter une image
description: Rubrique de référence pour Add-image, qui ajoute des images à un serveur des services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fb252fb5e10cc18d421c44d6edca893879905a5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721080"
---
# <a name="add-image"></a>Ajouter une image

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute des images à un serveur des services de déploiement Windows.

## <a name="syntax"></a>Syntaxe
pour les images de démarrage, utilisez la syntaxe suivante :
```
wdsutil /add-ImagmediaFile:<wim file path> [/Server:<Server name>mediatype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>] 
[/Filename:<New wim file name>]
```
pour les images d’installation, utilisez la syntaxe suivante :
```
wdsutil /add-ImagmediaFile:<wim file path>
     [/Server:<Server name>]
   mediatype:Install
     [/Skipverify]
    mediaGroup:<Image group name>]
     [/SingleImage:<Single image name>]
         [/Name:<Name>]
         [/Description:<Description>]
     [/Filename:<File name>]
     [/UnattendFile:<Unattend file path>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaFile : chemin du fichier <. wim>|Spécifie le chemin d’accès complet et le nom de fichier du fichier image Windows (. wim) qui contient les images à ajouter.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n'est spécifié, le serveur local est utilisé.|
MediaType : {Boot&#124;install}|Spécifie le type d’images à ajouter.|
|[/Skipverify]|Spécifie que la vérification de l’intégrité ne sera pas effectuée sur le fichier image source avant l’ajout de l’image.|
|[/Name :<Name>]|Définit le nom d’affichage de l’image.|
|/Description<Description>]|Définit la description de l’image.|
|[/Filename:<Filename>]|Spécifie le nouveau nom de fichier pour le fichier. wim. Cela vous permet de modifier le nom de fichier du fichier. wim lors de l’ajout de l’image. Si aucun nom de fichier n’est spécifié, le nom du fichier image source est utilisé. Dans tous les cas, les services de déploiement Windows vérifient si le nom de fichier est unique dans le magasin d’images de démarrage de l’ordinateur de destination.|
|\mediaGroup :<Image group name>]|Spécifie le nom du groupe d’images dans lequel les images doivent être ajoutées. Si plusieurs groupes d’images existent sur le serveur, le groupe d’images doit être spécifié. Si aucun groupe d'images n'est spécifié et s'il n'en existe pas déjà, un groupe d'images est créé. Dans le cas contraire, le groupe d'images existant est utilisé.|
|[/SingleImage :<Single image name>] [/Name :<Name>] [/Description :<Description>]|Copie l’image unique spécifiée à partir d’un fichier. wim et définit le nom d’affichage et la description de l’image.|
|[/UnattendFile:<Unattend file path>]|Spécifie le chemin d’accès complet au fichier d’installation sans assistance à associer aux images en cours d’ajout. Si **/SingleImage** n’est pas spécifié, le même fichier d’installation sans assistance est associé à toutes les images dans le fichier. wim.|
## <a name="examples"></a>Exemples
Pour ajouter une image de démarrage, tapez :
```
wdsutil /add-ImagmediaFile:C:\MyFolder\Boot.wimmediatype:Boot
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share\Boot.wim /Server:MyWDSServemediatype:Boot /Name:My WinPE Image 
/Description:WinPE Image containing the WDS Client /Filename:WDSBoot.wim
```
Pour ajouter une image d’installation, tapez l’une des options suivantes :
```
wdsutil /add-ImagmediaFile:C:\MyFolder\Install.wimmediatype:Install
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share \Install.wim /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/SingleImage:Windows Pro /Name:My WDS Image
/Description:Windows Pro image with Microsoft Office /Filename:Win Pro.wim /UnattendFile:\\server\share\unattend.xml
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l’aide[de la commande copy-image](using-the-copy-image-command.md)
à l’aide de la commande[Export-](using-the-export-image-command.md)
image à l’aide[de la commande](using-the-get-image-command.md)
-image à l’aide de la commande[Remove-](using-the-remove-image-command.md)
image à l’aide de la sous-commande de[commande](using-the-replace-image-command.md)
Replace-image[: Set-image](subcommand-set-image.md)
