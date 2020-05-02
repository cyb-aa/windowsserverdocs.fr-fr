---
title: New-MulticastTransmission
description: Rubrique de référence pour New-MulticastTransmission, qui crée une transmission par multidiffusion pour une image.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c1f1dc46-dd50-4eb9-9f72-cf0e5d71bd3d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 779dd0937f6889f795268bf59aa35837e84aca42
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720876"
---
# <a name="new-multicasttransmission"></a>New-MulticastTransmission

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une transmission par multidiffusion pour une image. Cette commande est équivalente à la création d’une transmission à l’aide du composant logiciel enfichable MMC des services de déploiement Windows (cliquez avec le bouton droit sur le nœud **transmissions par multidiffusion** , puis cliquez sur **créer une transmission par multidiffusion**). Vous devez utiliser cette commande lorsque le service de rôle serveur de déploiement et le service de rôle serveur de transport sont installés (ce qui correspond à l’installation par défaut). Si vous n’avez installé que le service de rôle serveur de transport, utilisez [la commande New-Namespace](using-the-new-namespace-command.md).
## <a name="syntax"></a>Syntaxe
pour les transmissions d’images d’installation :
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
pour les transmissions d’image de démarrage (uniquement prises en charge pour Windows Server 2008 R2) :
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
Media<Image name>|Spécifie le nom de l’image à transmettre à l’aide de la multidiffusion.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|Convivial<Friendly name>|Spécifie le nom convivial de la transmission.|
|/Description<Description>]|Spécifie la description de la transmission.|
MediaType : {Boot&#124;install}|Spécifie le type d’image à transmettre à l’aide de la multidiffusion. Remarque le **démarrage** est pris en charge uniquement pour Windows Server 2008 R2.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images qui contient l’image. Si aucun nom de groupe d’images n’est spécifié et qu’il n’existe qu’un seul groupe d’images sur le serveur, ce groupe d’images est utilisé. Si plusieurs groupes d’images existent sur le serveur, vous devez utiliser cette option pour spécifier le nom du groupe d’images.|
|[/Filename:<File name>]|Spécifie le nom du fichier. Si l’image source ne peut pas être identifiée de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom du fichier.|
|/TransmissionType : {AutoCast &#124; ScheduledCast}|Spécifie si la transmission doit être démarrée automatiquement (AutoCast) ou selon les critères de début spécifiés (ScheduledCast).<p><ul><li>**Conversion automatique**. Ce type de transmission indique que dès qu’un client applicable demande une image d’installation, une transmission par multidiffusion de l’image sélectionnée commence. Comme les autres clients demandent la même image, ils sont joints à la transmission qui a déjà démarré.</li><li>**Conversion planifiée**. Ce type de transmission définit les critères de démarrage de la transmission en fonction du nombre de clients qui demandent une image et/ou d’une date et d’une heure spécifiques. Vous pouvez spécifier les options suivantes :<p><ul><li>[/Time : <time>]-définit l’heure à laquelle la transmission doit commencer en utilisant le format suivant : aaaa/mm/jj : HH : mm.</li><li>[/Clients : <Number of clients>]-définit le nombre minimal de clients à attendre avant le démarrage de la transmission.</li></ul></li></ul>|
|/Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image de démarrage à transmettre à l’aide de la multidiffusion. Étant donné qu’il est possible d’avoir le même nom pour les images de démarrage de différentes architectures, vous devez spécifier l’architecture pour vous assurer que l’image correcte est utilisée.|
|[/Filename:<File name>]|Spécifie le nom du fichier. Si l’image source ne peut pas être identifiée de manière unique par son nom, vous devez spécifier le nom du fichier.|
## <a name="examples"></a>Exemples
Pour créer une transmission par diffusion automatique d’une image de démarrage dans Windows Server 2008 R2, tapez :
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS Boot Transmission
/Image:X64 Boot Imagemediatype:Boot /Architecture:x64 /Transmissiontype:AutoCast
```
Pour créer une transmission par diffusion automatique d’une image d’installation, tapez :
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS AutoCast Transmission
/Image:Vista with Officemediatype:Install /Transmissiontype:AutoCast
```
Pour créer une transmission par diffusion planifiée d’une image d’installation, tapez :
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS SchedCast Transmission /Server:MyWDSServemedia:Vista with Officemediatype:Install 
/Transmissiontype:ScheduledCast /time:2006/11/20:17:00 /Clients:100
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande[à l’aide de la commande](using-the-get-allmulticasttransmissions-command.md)
AllMulticastTransmissions à l’aide de la[commande](using-the-get-multicasttransmission-command.md)
MulticastTransmission à l’aide[de la commande Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
, sous-[commande : Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
