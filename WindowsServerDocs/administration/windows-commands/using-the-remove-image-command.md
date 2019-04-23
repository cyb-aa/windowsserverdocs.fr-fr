---
title: À l’aide de la commande remove-Image
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2aaf7d63d858045f9dd5df399c5f3f92b038bc2e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846700"
---
# <a name="using-the-remove-image-command"></a>À l’aide de la commande remove-Image

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime une image d’un serveur.
## <a name="syntax"></a>Syntaxe
images de démarrage :
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
média :<Image name>|Spécifie le nom de l’image.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
mediatype:{Boot &#124; Install}|Spécifie le type d’image.|
|/ Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image. Comme il est possible d’avoir le même nom d’image pour les images de démarrage différents dans différentes architectures, en spécifiant la valeur de l’architecture permet de s’assurer que l’image correcte sera supprimée.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images qui contient l’image. Si aucun nom de groupe d’image n’est spécifié et qu’une seule image existe sur le serveur, ce groupe d’images sera utilisé. Si plus d’un groupe d’images existe, vous devez utiliser cette option pour spécifier le groupe d’images.|
|[/Filename:<File name>]|Si l’image ne peut pas être identifié de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom de fichier.|
## <a name="BKMK_examples"></a>Exemples
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
[à l’aide de la commande add-Image](using-the-add-image-command.md)
[à l’aide de la commande de copie-Image](using-the-copy-image-command.md)
[à l’aide du Commande export-Image](using-the-export-image-command.md)
[à l’aide de la commande get-Image](using-the-get-image-command.md)
[à l’aide de l’Image de remplacement commande](using-the-replace-image-command.md) 
 [ Sous-commande : set-Image](subcommand-set-image.md)
