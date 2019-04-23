---
title: Sous-commande set-Image
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e63c67210764de76edae18a1897a68d763f9d695
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856480"
---
# <a name="subcommand-set-image"></a>Sous-commande : set-Image

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie les attributs d’une image.
## <a name="syntax"></a>Syntaxe
images de démarrage :
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
média :<Image name>|Spécifie le nom de l’image.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
mediatype:{Boot &#124; Install}|Spécifie le type d’image.|
|/ Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image. Étant donné que vous pouvez avoir le même nom d’image pour les images de démarrage différents dans différentes architectures, en spécifiant l’architecture permet de s’assurer que l’image correcte est modifié.|
|[/Filename:<File name>]|Si l’image ne peut pas être identifié de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom de fichier.|
|[/Name]|Spécifie le nom de l’image.|
|[/ Description :<Description>]|Définit la description de l’image.|
|[/ Enabled : {Oui &#124; No}]|Active ou désactive l’image.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images qui contient l’image. Si aucun nom de groupe d’image n’est spécifié et qu’une seule image existe sur le serveur, ce groupe d’images sera utilisé. Si plus d’un groupe d’images existe sur le serveur, vous devez utiliser cette option pour spécifier le groupe d’images.|
|[/ UserFilter :<SDDL>]|Définit le filtre de l’utilisateur sur l’image. La chaîne de filtre doit être au format de définition du langage SDDL (Security Descriptor). Notez que, contrairement à la **/sécurité** option pour les groupes de l’image, cette option restreint uniquement qui peut voir la définition de l’image et pas les ressources de fichier image réelle. Pour restreindre l’accès aux ressources de fichier et par conséquent accéder à toutes les images au sein d’un groupe d’images, vous devez définir la sécurité pour le groupe d’images.|
|[/UnattendFile:<Unattend file path>]|Définit le chemin d’accès complet au fichier à associer à l’image d’installation sans assistance. Exemple : **D:\Files\Unattend\Img1Unattend.xml**|
|[/ OverwriteUnattend : {Oui &#124; No}]|Vous pouvez spécifier **/overwrite** pour remplacer le fichier d’installation sans assistance s’il existe déjà un fichier d’installation sans assistance associé à l’image. Notez que le paramètre par défaut est **non**.|
## <a name="BKMK_examples"></a>Exemples
Pour définir des valeurs pour une image de démarrage, tapez une des opérations suivantes :
```
wdsutil /Set-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /Description:"New description"
wdsutil /verbose /Set-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim 
/Name:"New Name" /Description:"New Description" /Enabled:Yes
```
Pour définir des valeurs pour une image d’installation, tapez une des opérations suivantes :
```
wdsutil /Set-Imagmedia:"Windows Vista with Officemediatype:Install /Description:"New description" 
wdsutil /verbose /Set-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /Name:"New name" /Description:"New description" /UserFilter:"O:BAG:DUD:AI(A;ID;FA;;;SY)(A;ID;FA;;;BA)(A;ID;0x1200a9;;;AU)" /Enabled:Yes /UnattendFile:\\server\share\unattend.xml /OverwriteUnattend:Yes
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-Image](using-the-add-image-command.md)
[à l’aide de la commande de copie-Image](using-the-copy-image-command.md)
[à l’aide du Commande export-Image](using-the-export-image-command.md)
[à l’aide de la commande get-Image](using-the-get-image-command.md)
[à l’aide de la commande remove-Image](using-the-remove-image-command.md) 
 [ À l’aide de la commande replace-Image](using-the-replace-image-command.md)
