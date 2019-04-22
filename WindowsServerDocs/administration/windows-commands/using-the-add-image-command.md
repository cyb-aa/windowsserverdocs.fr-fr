---
title: À l’aide de la commande add-Image
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0433e0775bd2088170ae17fcfe432cdaee0bf99d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817460"
---
# <a name="using-the-add-image-command"></a>À l’aide de la commande add-Image

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute des images à un serveur de Services de déploiement Windows. Pour obtenir des exemples de la façon dont vous pouvez utiliser cette commande, consultez [exemples](#BKMK_examples).
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
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaFile : < chemin d’accès du fichier .wim >|Spécifie le chemin d’accès et le nom complet du fichier Image Windows (.wim) qui contient les images à ajouter.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n'est spécifié, le serveur local est utilisé.|
mediatype:{Boot&#124;Install}|Spécifie le type d’images à ajouter.|
|[/Skipverify]|Spécifie que vérification de l’intégrité ne sera pas effectuée sur le fichier image source avant que l’image est ajoutée.|
|[/ Nom :<Name>]|Définit le nom complet de l’image.|
|[/ Description :<Description>]|Définit la description de l’image.|
|[/Filename:<Filename>]|Spécifie le nouveau nom de fichier pour le fichier .wim. Cela vous permet de modifier le nom de fichier du fichier .wim lors de l’ajout de l’image. Si aucun nom de fichier n’est spécifié, le nom du fichier image source sera utilisé. Dans tous les cas, les Services de déploiement Windows vérifie pour déterminer si le nom de fichier est unique dans le magasin d’images de démarrage de l’ordinateur de destination.|
|\mediaGroup :<Image group name>]|Spécifie le nom de groupe d’images dans lequel les images doivent être ajoutées. Si plus d’un groupe d’images existe sur le serveur, vous devez spécifier le groupe d’images. Si aucun groupe d'images n'est spécifié et s'il n'en existe pas déjà, un groupe d'images est créé. Dans le cas contraire, le groupe d'images existant est utilisé.|
|[/ SingleImage :<Single image name>] [/ nom :<Name>] [/ Description :<Description>]|Copie l’image unique spécifiée en dehors d’un fichier .wim et définit le nom complet de l’image et description.|
|[/UnattendFile:<Unattend file path>]|Spécifie le chemin complet vers le fichier d’installation sans assistance à associer avec les images qui sont ajoutés. Si **/SingleImage** n’est pas spécifié, le même fichier d’installation sans assistance sera associé à toutes les images dans le fichier .wim.|
## <a name="BKMK_examples"></a>Exemples
Pour ajouter une image de démarrage, tapez :
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Boot.wimmediatype:Boot
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share\Boot.wim /Server:MyWDSServemediatype:Boot /Name:"My WinPE Image" 
/Description:"WinPE Image containing the WDS Client" /Filename:WDSBoot.wim
```
Pour ajouter une image d’installation, tapez une des opérations suivantes :
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Install.wimmediatype:Install
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share \Install.wim /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/SingleImage:"Windows Pro" /Name:"My WDS Image"
/Description:"Windows Pro image with Microsoft Office" /Filename:"Win Pro.wim" /UnattendFile:"\\server\share\unattend.xml"
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande de copie-Image](using-the-copy-image-command.md)
[à l’aide de la commande Export-Image](using-the-export-image-command.md)
[à l’aide du Get-Image commande](using-the-get-image-command.md)
[à l’aide de la commande remove-Image](using-the-remove-image-command.md)
[à l’aide de l’Image de remplacement commande](using-the-replace-image-command.md) 
 [ Sous-commande : set-Image](subcommand-set-image.md)
