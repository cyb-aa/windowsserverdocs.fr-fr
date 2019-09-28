---
title: Utilisation de la commande Remove-MulticastTransmission
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 279554124b046f645b3c83e1490657aa8782104a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362820"
---
# <a name="using-the-remove-multicasttransmission-command"></a>Utilisation de la commande Remove-MulticastTransmission

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Désactive la transmission par multidiffusion pour une image. À moins que vous ne spécifiiez **/force**, les clients existants effectuent le transfert d’image, mais les nouveaux clients ne sont pas autorisés à se joindre.
## <a name="syntax"></a>Syntaxe
**Windows Server 2008**
```
wdsutil /remove-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image Group>] [/Filename:<File name>] [/force]
```
**Windows Server 2008 R2** pour les images de démarrage :
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
\x20    [/Server:<Server name>]
\x20  mediatype:Boot
\x20    /Architecture:{x86 | ia64 | x64}
\x20    [/Filename:<File name>]
```
pour les images d’installation :
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group
        [/Filename:<File name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média : <Image name>|Spécifie le nom de l’image.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : {install&#124;Boot}|Spécifie le type d’image. Notez que cette option doit être définie sur **installer** pour Windows Server 2008.|
|/Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image de démarrage associée à la transmission à démarrer. Étant donné qu’il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, vous devez spécifier l’architecture pour vous assurer que la transmission correcte est utilisée.|
|\mediaGroup : <Image group name>]|Spécifie le groupe d’images qui contient l’image. Si aucun nom de groupe d’images n’est spécifié et qu’il n’existe qu’un seul groupe d’images sur le serveur, ce groupe d’images est utilisé. Si plusieurs groupes d’images existent sur le serveur, vous devez utiliser cette option pour spécifier le nom du groupe d’images.|
|[/Filename:<File name>]|Spécifie le nom du fichier. Si l’image source ne peut pas être identifiée de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom du fichier.|
|/Force|supprime la transmission et met fin à tous les clients. À moins que vous ne spécifiiez une valeur pour l’option **/force** , les clients existants peuvent effectuer le transfert d’image, mais les nouveaux clients ne peuvent pas se joindre.|
## <a name="BKMK_examples"></a>Illustre
Pour arrêter un espace de noms (les clients actuels terminent la transmission, mais les nouveaux clients ne peuvent pas se joindre à), tapez :
```
wdsutil /remove-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmissiomedia:"x64 Boot Image"
/Imagetype:Boot /Architecture:x64
```
Pour forcer l’arrêt de tous les clients, tapez :
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[à l’aide de la commande MulticastTransmission](using-the-get-multicasttransmission-command.md)
[à l’aide de la commande New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
[ Sous-commande : Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
