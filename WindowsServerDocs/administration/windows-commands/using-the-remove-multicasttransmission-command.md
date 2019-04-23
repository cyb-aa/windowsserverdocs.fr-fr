---
title: À l’aide de la commande remove-MulticastTransmission
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: dc3ba385644ef9da9b5d592142091ff087cd7545
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839680"
---
# <a name="using-the-remove-multicasttransmission-command"></a>À l’aide de la commande remove-MulticastTransmission

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Désactive la transmission multidiffusion pour une image. Sauf si vous spécifiez **/force**, les clients existants seront termine le transfert de l’image mais les nouveaux clients ne pourrez pas joindre.
## <a name="syntax"></a>Syntaxe
**Windows Server 2008**
```
wdsutil /remove-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image Group>] [/Filename:<File name>] [/force]
```
**Windows Server 2008 R2** des images de démarrage :
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
média :<Image name>|Spécifie le nom de l’image.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
mediatype:{Install&#124;Boot}|Spécifie le type d’image. Notez que cette option doit être définie sur **installer** pour Windows Server 2008.|
|/ Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image de démarrage qui est associé à la transmission à démarrer. Comme il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, vous devez spécifier l’architecture pour vous assurer que la transmission correcte est utilisée.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images qui contient l’image. Si aucun nom de groupe d’image n’est spécifié et qu’une seule image existe sur le serveur, ce groupe d’images est utilisé. Si plus d’un groupe d’images existe sur le serveur, vous devez utiliser cette option pour spécifier le nom de groupe d’image.|
|[/Filename:<File name>]|Spécifie le nom de fichier. Si l’image source ne peut pas être identifié de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom de fichier.|
|[/force]|Supprime de la transmission et met fin à tous les clients. Sauf si vous spécifiez une valeur pour le **/force** option, existants des clients peut terminer le transfert de l’image, mais les nouveaux clients ne sont pas en mesure de joindre.|
## <a name="BKMK_examples"></a>Exemples
Pour arrêter un espace de noms (les clients en cours seront termine la transmission, mais les nouveaux clients ne seront pas en mesure de joindre), type :
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
[à l’aide de la commande get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[à l’aide de la commande get-MulticastTransmission](using-the-get-multicasttransmission-command.md) 
 [à l’aide de la commande Nouveau MulticastTransmission](using-the-new-multicasttransmission-command.md)
[sous-commande : start-MulticastTransmission](subcommand-start-multicasttransmission.md)
