---
title: Utilisation de la commande Remove-image
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce5e2384-2264-4b22-92af-74eec8c10ae0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0876649a4de55604707f03ca98cf0ceed2920b9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362829"
---
# <a name="using-the-remove-image-command"></a>Utilisation de la commande Remove-image

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

supprime une image d’un serveur.
## <a name="syntax"></a>Syntaxe
pour les images de démarrage :
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<Filename>]
```
pour les images d’installation :
```
wdsutil [Options] /remove-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<Filename>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média : <Image name>|Spécifie le nom de l’image.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : {boot &#124; install}|Spécifie le type d’image.|
|/Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image. Étant donné qu’il est possible d’avoir le même nom d’image pour différentes images de démarrage dans différentes architectures, la spécification de la valeur d’architecture garantit que l’image correcte sera supprimée.|
|\mediaGroup : <Image group name>]|Spécifie le groupe d’images qui contient l’image. Si aucun nom de groupe d’images n’est spécifié et qu’il n’existe qu’un seul groupe d’images sur le serveur, ce groupe d’images sera utilisé. Si plusieurs groupes d’images existent, vous devez utiliser cette option pour spécifier le groupe d’images.|
|[/Filename:<File name>]|Si l’image ne peut pas être identifiée de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom du fichier.|
## <a name="BKMK_examples"></a>Illustre
Pour supprimer une image de démarrage, tapez :
```
wdsutil /remove-Imagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86
```
```
wdsutil /verbose /remove-Imagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim
```
Pour supprimer une image d’installation, tapez :
```
wdsutil /remove-Imagmedia:"Windows Vista with Officemediatype:Install
```
```
wdsutil /verbose /remove-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-image](using-the-add-image-command.md)
[à l’aide de la commande copy-image](using-the-copy-image-command.md)
[à l’aide de la commande Export-image](using-the-export-image-command.md)
[à l’aide de la commande d’extraction d’image](using-the-get-image-command.md)
 à l'[aide de l’option Commande Replace-image](using-the-replace-image-command.md)1 sous-[commande : Set-image](subcommand-set-image.md)
