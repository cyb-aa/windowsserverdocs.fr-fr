---
title: bootcfg debug
description: Rubrique de commandes de Windows pour **bootcfg débogage** - ajoute ou modifie les paramètres de débogage pour une entrée de système d’exploitation spécifié.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27768788e7f14445137331523c62151fcf3b6b2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879020"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute ou modifie les paramètres de débogage pour une entrée de système d’exploitation spécifié.

## <a name="syntax"></a>Syntaxe
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|{ON &#124; OFF&#124; edit}|Spécifie la valeur pour le débogage.<br /><br />**ON** -permet la prise en charge du débogage à distance en ajoutant l’option /debug spécifié <OSEntryLineNum>.<br /><br />**DÉSACTIVER** -désactive la prise en charge du débogage à distance en supprimant l’option /debug à partir du spécifié <OSEntryLineNum>.<br /><br />**modifier** -permet des paramètres de la fréquence des modifications port et le débit en modifiant les valeurs associées à l’option /debug spécifié <OSEntryLineNum>.|
|/s <computer>|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u <Domain>\\<User>|Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain> \\ <User>. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande.|
|/p <Password>|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|/port {COM1 &#124; COM2 &#124; COM3 &#124; COM4}|Spécifie le port COM à utiliser pour le débogage. N’utilisez pas le **portd’insertion/** paramètre si le débogage est désactivé.|
|/baud {9600&#124; 19 200 bauds&#124; 38400&#124; 57600&#124; 115200}|Spécifie la vitesse de transmission à utiliser pour le débogage. N’utilisez pas le **/baud** paramètre si le débogage est désactivé.|
|/id <OSEntryLineNum>|Spécifie le numéro de ligne d’entrée système d’exploitation dans la section [operating systems] du fichier Boot.ini auquel sont ajoutées les options de débogage. La première ligne après l’en-tête de section de la section [operating systems] est 1.|
|/?|Affiche l'aide à l'invite de commandes.|
##### <a name="remarks"></a>Notes
-   Si le débogage de port 1394 est requis, utilisez [bootcfg dbg1394](bootcfg-dbg1394.md).
## <a name="BKMK_examples"></a>Exemples
Les exemples suivants montrent comment vous pouvez utiliser la **/Debug bootcfg**commande :
```
bootcfg /debug on /port com1 /id 2 
bootcfg /debug edit /port com2 /baud 19200 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
