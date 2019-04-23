---
title: Sous-commande start-MulticastTransmission
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1b2d459-1ece-49d4-997c-9d206c463b61
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7e3e59a0907caf2769d5df00aeaf00589ab450d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842680"
---
# <a name="subcommand-start-multicasttransmission"></a>Sous-commande : start-MulticastTransmission

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

démarre une transmission par diffusion planifiée d’une image.
## <a name="syntax"></a>Syntaxe
**Windows Server 2008**
```
wdsutil /start-MulticastTransmissiomedia:<Image name> [/Server:<Server namemediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
**Windows Server 2008 R2** des images de démarrage :
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
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média :<Image name>|Spécifie le nom de l’image.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
mediatype:{Install&#124;Boot}|Spécifie le type d’image. Notez que cette option doit être définie sur **installer** pour Windows Server 2008.|
|/ Architecture : {x86 &#124; ia64 &#124; x64}|L’architecture de l’image de démarrage qui est associé à la transmission à démarrer. Dans la mesure où il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, vous devez spécifier l’architecture pour vous assurer que la transmission correcte est utilisée.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images de l’image. Si aucun nom de groupe d’image n’est spécifié et qu’une seule image existe sur le serveur, ce groupe d’images sera utilisé. Si plus d’un groupe d’images existe sur le serveur, vous devez utiliser cette option pour spécifier le nom de groupe d’image.|
|[/Filename:<File name>]|Spécifie le nom du fichier qui contient l’image. Si l’image ne peut pas être identifié de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom de fichier.|
## <a name="BKMK_examples"></a>Exemples
Pour démarrer une transmission par multidiffusion, tapez une des opérations suivantes :
```
wdsutil /start-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
Pour lancer une transmission par multidiffusion pour Windows Server 2008 R2, type d’image :
```
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"X64 Boot Imagemediatype:Boot /Architecture:x64
/Filename:boot.wim\n\
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[à l’aide de la commande get-MulticastTransmission](using-the-get-multicasttransmission-command.md) 
 [à l’aide de la commande Nouveau MulticastTransmission](using-the-new-multicasttransmission-command.md)
[à l’aide de la commande remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
