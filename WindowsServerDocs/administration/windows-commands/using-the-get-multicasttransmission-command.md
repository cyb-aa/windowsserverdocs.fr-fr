---
title: MulticastTransmission
description: La rubrique commandes Windows pour la commande MulticastTransmission, qui affiche des informations sur la transmission par multidiffusion pour une image spécifiée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b733737b-1e81-43d4-a058-d6985a613bef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec66107452be86423a4d542ee3999e86f1446656
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830942"
---
# <a name="get-multicasttransmission"></a>MulticastTransmission

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur la transmission par multidiffusion pour une image spécifiée.

## <a name="syntax"></a>Syntaxe
**Windows Server 2008**
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] 
[/Filename:<File name>] [/Show:Clients]
```
**Windows Server 2008 R2** pour les transmissions d’image de démarrage :
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
    [/Server:<Server name>]
    [/details:Clients]
  mediatype:Boot
    /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
pour les transmissions d’image d’installation :
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
         [/Server:<Server name>]
         [/details:Clients]
       mediatype:Install
    mediaGroup:<Image Group>]
     [/Filename:<File name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média :<Image name>|Affiche la transmission par multidiffusion associée à cette image.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
MediaType : installation|Spécifie le type d’image. Notez que cette option doit être définie sur **installer**.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images qui contient l’image. Si aucun nom de groupe d’images n’est spécifié et qu’il n’existe qu’un seul groupe d’images sur le serveur, ce groupe d’images est utilisé. Si plusieurs groupes d’images existent sur le serveur, vous devez utiliser cette option pour spécifier un groupe d’images.|
|/Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image de démarrage associée à la transmission. Étant donné qu’il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, vous devez spécifier l’architecture pour vous assurer que l’image correcte est utilisée.|
|[/Filename:<File name>]|Spécifie le fichier qui contient l’image. Si l’image ne peut pas être identifiée de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom du fichier.|
|[/Show : clients]<p>ou<p>[/details : clients]|Affiche des informations sur les ordinateurs clients qui sont connectés à la transmission par multidiffusion.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
**Windows Server 2008** Pour afficher des informations sur la transmission d’une image nommée Vista avec Office, tapez l’une des options suivantes :
```
wdsutil /Get-MulticastTransmissiomedia:Vista with Officemediatype:Install
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /Show:Clients
```
**Windows Server 2008 R2** Pour afficher des informations sur la transmission d’une image nommée Vista avec Office, tapez l’une des options suivantes :
```
wdsutil /Get-MulticastTransmissiomedia:Vista with Office
 /Imagetype:Install
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /details:Clients
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:X64 Boot Imagemediatype:Boot /Architecture:x64 /Filename:boot.wim /details:Clients
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[à l’aide de la commande New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
[à l’aide de la commande Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
sous- [commande : Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
