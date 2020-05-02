---
title: Sous-commande Set-image
description: Rubrique de référence pour la sous-commande Set-image, qui modifie les attributs d’une image.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ae03c86-7a13-4e38-9182-32e55fffd504
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e24b20093a726e7553474871ef25e6877223e21f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721712"
---
# <a name="subcommand-set-image"></a>Sous-commande : Set-image

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie les attributs d’une image.

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
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
Media<Image name>|Spécifie le nom de l'image.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : {Boot &#124; install}|Spécifie le type d’image.|
|/Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image. Étant donné que vous pouvez avoir le même nom d’image pour différentes images de démarrage dans différentes architectures, la spécification de l’architecture garantit la modification de l’image correcte.|
|[/Filename:<File name>]|Si l’image ne peut pas être identifiée de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom du fichier.|
|/Name|Spécifie le nom de l'image.|
|/Description<Description>]|Définit la description de l’image.|
|[/Enabled : {Oui &#124; non}]|Active ou désactive l’image.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images qui contient l’image. Si aucun nom de groupe d’images n’est spécifié et qu’il n’existe qu’un seul groupe d’images sur le serveur, ce groupe d’images sera utilisé. Si plusieurs groupes d’images existent sur le serveur, vous devez utiliser cette option pour spécifier le groupe d’images.|
|[/UserFilter :<SDDL>]|Définit le filtre utilisateur sur l’image. La chaîne de filtrage doit être au format SDDL (Security Descriptor Definition Language). Notez que, contrairement à l’option **/Security** pour les groupes d’images, cette option restreint uniquement qui peut voir la définition de l’image, et non les ressources du fichier image réel. Pour restreindre l’accès aux ressources de fichier et, par conséquent, l’accès à toutes les images dans un groupe d’images, vous devez définir la sécurité pour le groupe d’images lui-même.|
|[/UnattendFile:<Unattend file path>]|Définit le chemin d’accès complet au fichier d’installation sans assistance à associer à l’image. Par exemple : **D:\Files\Unattend\Img1Unattend.xml**|
|[/OverwriteUnattend : {Oui &#124; non}]|Vous pouvez spécifier **/overwrite** pour remplacer le fichier d’installation sans assistance Si un fichier d’installation sans assistance est déjà associé à l’image. Notez que le paramètre par défaut est **non**.|
## <a name="examples"></a>Exemples
Pour définir les valeurs d’une image de démarrage, tapez l’une des valeurs suivantes :
```
wdsutil /Set-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86 /Description:New description
wdsutil /verbose /Set-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim 
/Name:New Name /Description:New Description /Enabled:Yes
```
Pour définir les valeurs d’une image d’installation, tapez l’une des options suivantes :
```
wdsutil /Set-Imagmedia:Windows Vista with Officemediatype:Install /Description:New description 
wdsutil /verbose /Set-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /Name:New name /Description:New description /UserFilter:O:BAG:DUD:AI(A;ID;FA;;;SY)(A;ID;FA;;;BA)(A;ID;0x1200a9;;;AU) /Enabled:Yes /UnattendFile:\\server\share\unattend.xml /OverwriteUnattend:Yes
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l’aide[de la commande Add-image](using-the-add-image-command.md)
[à l’aide de la commande](using-the-copy-image-command.md)
copy-image à l’aide de la commande[Export-](using-the-export-image-command.md)
image à l’aide[de](using-the-get-image-command.md)
la commande-image à l’aide de la commande[Remove](using-the-remove-image-command.md)
-image à l'[aide de la commande Replace-image](using-the-replace-image-command.md)
