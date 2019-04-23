---
title: À l’aide de la commande Nouveau MulticastTransmission
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1f1dc46-dd50-4eb9-9f72-cf0e5d71bd3d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84f5aa69f2d4a875995ac6c18fa43bd68518fba3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875140"
---
# <a name="using-the-new-multicasttransmission-command"></a>À l’aide de la commande Nouveau MulticastTransmission

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une nouvelle transmission par multidiffusion pour une image. Cette commande est équivalente à la création d’une transmission à l’aide du composant logiciel enfichable mmc Services de déploiement Windows (avec le bouton droit le **des transmissions par multidiffusion** nœud, puis cliquez sur **créer Transmission par multidiffusion**). Vous devez utiliser cette commande une fois le service de rôle de serveur de déploiement et le service de rôle serveur de Transport installé (c'est-à-dire l’installation par défaut). Si vous avez uniquement le service de rôle de serveur de Transport installé, utilisez [à l’aide de la commande Nouveau Namespace](using-the-new-namespace-command.md).
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
pour les transmissions d’image de démarrage (uniquement pris en charge pour Windows Server 2008 R2) :
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
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
média :<Image name>|Spécifie le nom de l’image doit être transmis à l’aide de la multidiffusion.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
|/ FriendlyName :<Friendly name>|Spécifie le nom convivial de la transmission.|
|[/ Description :<Description>]|Spécifie la description de la transmission.|
mediatype:{Boot&#124;Install}|Spécifie le type d’image doit être transmis à l’aide de la multidiffusion. Remarque **démarrage** est uniquement pris en charge pour Windows Server 2008 R2.|
|\mediaGroup :<Image group name>]|Spécifie le groupe d’images qui contient l’image. Si aucun nom de groupe d’image n’est spécifié et qu’une seule image existe sur le serveur, ce groupe d’images est utilisé. Si plus d’un groupe d’images existe sur le serveur, vous devez utiliser cette option pour spécifier le nom de groupe d’image.|
|[/Filename:<File name>]|Spécifie le nom de fichier. Si l’image source ne peut pas être identifié de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom de fichier.|
|/Transmissiontype:{AutoCast &#124; ScheduledCast}|Spécifie s’il faut démarrer automatiquement de la transmission (diffusion) ou selon les critères de début spécifiée (par diffusion planifiée).<br /><br /><ul><li>**Diffusion automatique**. Ce type de transmission indique que, dès qu’un client applicable demande une image d’installation, une transmission par multidiffusion de l’image sélectionnée commence. Comme d’autres clients qui demandent la même image, elles sont jointes à la transmission est déjà démarrée.</li><li>**La diffusion planifiée**. Ce type de transmission définit les critères de début pour la transmission en fonction du nombre de clients qui demandent une image et/ou une date et heure spécifique. Vous pouvez spécifier les options suivantes :<br /><br /><ul><li>[/ heure : <time>]-définit l’heure à laquelle la transmission doit démarrer en utilisant le format suivant : AAAA/MM/DD:hh:mm.</li><li>[/ Clients : <Number of clients>]-définit le nombre minimal de clients à attendre avant le démarrage de la transmission.</li></ul></li></ul>|
|/ Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image de démarrage à transmettre à l’aide de la multidiffusion. Comme il est possible d’avoir le même nom pour les images de démarrage de différentes architectures, vous devez spécifier l’architecture pour garantir que l’image correcte est utilisée.|
|[/Filename:<File name>]|Spécifie le nom de fichier. Si l’image source ne peut pas être identifié de manière unique par son nom, vous devez spécifier le nom de fichier.|
## <a name="BKMK_examples"></a>Exemples
Pour créer une transmission par diffusion automatique d’une image de démarrage dans Windows Server 2008 R2, tapez :
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS Boot Transmission"
/Image:"X64 Boot Imagemediatype:Boot /Architecture:x64 /Transmissiontype:AutoCast
```
Pour créer une transmission par diffusion automatique d’une image d’installation, tapez :
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS AutoCast Transmission"
/Image:"Vista with Officemediatype:Install /Transmissiontype:AutoCast
```
Pour créer une transmission par diffusion planifiée d’une image d’installation, tapez :
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS SchedCast Transmission" /Server:MyWDSServemedia:"Vista with Officemediatype:Install 
/Transmissiontype:ScheduledCast /time:"2006/11/20:17:00" /Clients:100
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[à l’aide de la commande get-MulticastTransmission](using-the-get-multicasttransmission-command.md) 
 [à l’aide de la commande remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
[sous-commande : start-MulticastTransmission](subcommand-start-multicasttransmission.md)
