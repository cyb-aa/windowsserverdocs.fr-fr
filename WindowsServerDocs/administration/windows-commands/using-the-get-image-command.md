---
title: Utilisation de la commande d’extraction d’image
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73b25fd70c1512f99b5097b2317c3f0403f3526c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363108"
---
# <a name="using-the-get-image-command"></a>Utilisation de la commande d’extraction d’image

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Récupère des informations sur une image.
## <a name="syntax"></a>Syntaxe
pour les images de démarrage :
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
pour les images d’installation :
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média : <Image name>|Spécifie le nom de l’image.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : {boot &#124; install}|Spécifie le type d’image.|
|/Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image. Étant donné qu’il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, la spécification de la valeur d’architecture garantit que l’image correcte est retournée.|
|[/Filename:<File name>]|Si l’image ne peut pas être identifiée de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom du fichier.|
|\mediaGroup : <Image group name>]|Spécifie le groupe d’images qui contient l’image. Si aucun groupe d’images n’est spécifié et qu’il n’existe qu’un seul groupe d’images sur le serveur, ce groupe est utilisé. Si plusieurs groupes d’images existent sur le serveur, vous devez utiliser ce paramètre pour spécifier le groupe d’images.|
## <a name="BKMK_examples"></a>Illustre
Pour récupérer des informations sur une image de démarrage, tapez l’une des options suivantes :
```
wdsutil /Get-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86
wdsutil /verbose /Get-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
```
Pour récupérer des informations sur une image d’installation, tapez l’une des options suivantes :
```
wdsutil /Get-Imagmedia:"Windows Vista with Officemediatype:Install
wdsutil /verbose /Get-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-image](using-the-add-image-command.md)
[à l’aide de la commande copy-image](using-the-copy-image-command.md)
[à l’aide de la commande Export-image](using-the-export-image-command.md)
[à l’aide de la commande Remove-Image](using-the-remove-image-command.md)
[à l’aide de l’option Commande Replace-image](using-the-replace-image-command.md)1 sous-[commande : Set-image](subcommand-set-image.md)
