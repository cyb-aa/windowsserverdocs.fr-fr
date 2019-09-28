---
title: Sous-commande Set-image
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ae03c86-7a13-4e38-9182-32e55fffd504
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4584bd6253b1991aba7e87fc42ff484101681081
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383856"
---
# <a name="subcommand-set-image"></a>Sous-commande : Set-image

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

modifie les attributs d’une image.
## <a name="syntax"></a>Syntaxe
pour les images de démarrage :
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] [/Name:<Name>] 
[/Description:<Description>] [/Enabled:{Yes | No}]
```
pour les images d’installation :
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     [/Name:<Name>]
     [/Description:<Description>]
     [/UserFilter:<SDDL>]
     [/Enabled:{Yes | No}]
     [/UnattendFile:<Unattend file path>]
         [/OverwriteUnattend:{Yes | No}]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média : <Image name>|Spécifie le nom de l’image.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : {boot &#124; install}|Spécifie le type d’image.|
|/Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image. Étant donné que vous pouvez avoir le même nom d’image pour différentes images de démarrage dans différentes architectures, la spécification de l’architecture garantit la modification de l’image correcte.|
|[/Filename:<File name>]|Si l’image ne peut pas être identifiée de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom du fichier.|
|/Name|Spécifie le nom de l’image.|
|/Description<Description>]|Définit la description de l’image.|
|[/Enabled : {oui &#124; non}]|Active ou désactive l’image.|
|\mediaGroup : <Image group name>]|Spécifie le groupe d’images qui contient l’image. Si aucun nom de groupe d’images n’est spécifié et qu’il n’existe qu’un seul groupe d’images sur le serveur, ce groupe d’images sera utilisé. Si plusieurs groupes d’images existent sur le serveur, vous devez utiliser cette option pour spécifier le groupe d’images.|
|[/UserFilter : <SDDL>]|Définit le filtre utilisateur sur l’image. La chaîne de filtrage doit être au format SDDL (Security Descriptor Definition Language). Notez que, contrairement à l’option **/Security** pour les groupes d’images, cette option restreint uniquement qui peut voir la définition de l’image, et non les ressources du fichier image réel. Pour restreindre l’accès aux ressources de fichier et, par conséquent, l’accès à toutes les images dans un groupe d’images, vous devez définir la sécurité pour le groupe d’images lui-même.|
|[/UnattendFile:<Unattend file path>]|Définit le chemin d’accès complet au fichier d’installation sans assistance à associer à l’image. Exemple : **D:\Files\Unattend\Img1Unattend.xml**|
|[/OverwriteUnattend : {oui &#124; non}]|Vous pouvez spécifier **/overwrite** pour remplacer le fichier d’installation sans assistance Si un fichier d’installation sans assistance est déjà associé à l’image. Notez que le paramètre par défaut est **non**.|
## <a name="BKMK_examples"></a>Illustre
Pour définir les valeurs d’une image de démarrage, tapez l’une des valeurs suivantes :
```
wdsutil /Set-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /Description:"New description"
wdsutil /verbose /Set-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim 
/Name:"New Name" /Description:"New Description" /Enabled:Yes
```
Pour définir les valeurs d’une image d’installation, tapez l’une des options suivantes :
```
wdsutil /Set-Imagmedia:"Windows Vista with Officemediatype:Install /Description:"New description" 
wdsutil /verbose /Set-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /Name:"New name" /Description:"New description" /UserFilter:"O:BAG:DUD:AI(A;ID;FA;;;SY)(A;ID;FA;;;BA)(A;ID;0x1200a9;;;AU)" /Enabled:Yes /UnattendFile:\\server\share\unattend.xml /OverwriteUnattend:Yes
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-image](using-the-add-image-command.md)
[à l’aide de la commande copy-image](using-the-copy-image-command.md)
[à l’aide de la commande Export-image](using-the-export-image-command.md)
[à l’aide de la commande d’extraction d’image](using-the-get-image-command.md)
 à l'[aide de l’option Commande Remove-Image](using-the-remove-image-command.md)1[à l’aide de la commande Replace-image](using-the-replace-image-command.md)
