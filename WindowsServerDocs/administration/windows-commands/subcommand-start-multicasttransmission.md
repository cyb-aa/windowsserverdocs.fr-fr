---
title: Sous-commande Start-MulticastTransmission
description: La rubrique commandes Windows pour la sous-commande Start-MulticastTransmission, qui démarre une transmission planifiée d’une image.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a1b2d459-1ece-49d4-997c-9d206c463b61
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e882c54d2fbe744ca9fe25b2631f4d875886c756
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833782"
---
# <a name="subcommand-start-multicasttransmission"></a>Sous-commande : Start-MulticastTransmission

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Démarre une transmission planifiée d’une image.

## <a name="syntax"></a>Syntaxe
**Windows Server 2008**
```
wdsutil /start-MulticastTransmissiomedia:<Image name> [/Server:<Server namemediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
**Windows Server 2008 R2** pour les images de démarrage :
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
pour les images d’installation :
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média :<Image name>|Spécifie le nom de l'image.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : {install&#124;Boot}|Spécifie le type d’image. Notez que cette option doit être définie sur **installer** pour Windows Server 2008.|
|/Architecture : {x86 &#124; ia64 &#124; x64}|Architecture de l’image de démarrage associée à la transmission à démarrer. Étant donné qu’il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, vous devez spécifier l’architecture pour vous assurer que la transmission correcte est utilisée.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images de l’image. Si aucun nom de groupe d’images n’est spécifié et qu’il n’existe qu’un seul groupe d’images sur le serveur, ce groupe d’images sera utilisé. Si plusieurs groupes d’images existent sur le serveur, vous devez utiliser cette option pour spécifier le nom du groupe d’images.|
|[/Filename:<File name>]|Spécifie le nom du fichier qui contient l’image. Si l’image ne peut pas être identifiée de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom du fichier.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour démarrer une transmission par multidiffusion, tapez l’une des options suivantes :
```
wdsutil /start-MulticastTransmissiomedia:Vista with Office
/Imagetype:Install
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
Pour démarrer une transmission par multidiffusion d’image de démarrage pour Windows Server 2008 R2, tapez :
```
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:X64 Boot Imagemediatype:Boot /Architecture:x64
/Filename:boot.wim\n\
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[à l’aide de la commande MulticastTransmission](using-the-get-multicasttransmission-command.md)
à l’aide de la commande [New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
[à l’aide de la commande Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
