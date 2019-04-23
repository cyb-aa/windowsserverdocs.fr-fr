---
title: bootcfg ems
description: Rubrique de commandes de Windows pour **bootcfg ems** -permet à l’utilisateur Ajouter ou modifier les paramètres de redirection de la console de gestion des Services d’urgence à un ordinateur distant.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97f0126fdfa4d800692a044d33e3de0f0d4c6cdb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861640"
---
# <a name="bootcfg-ems"></a>bootcfg ems

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permet à l’utilisateur Ajouter ou modifier les paramètres de redirection de la console de gestion des Services d’urgence à un ordinateur distant. En activant les Services de gestion d’urgence, vous ajoutez un « redirection = Port # « ligne à la section [boot loader] du fichier Boot.ini et une option /redirect à la ligne d’entrée de système d’exploitation spécifié. La fonctionnalité Services de gestion d’urgence est activée uniquement sur les serveurs.

## <a name="syntax"></a>Syntaxe
```
bootcfg /ems {ON | OFF | edit} [/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|{ON &#124; OFF&#124; edit}|Spécifie la valeur pour la redirection des Services de gestion d’urgence.<br /><br />**ON** -Active la sortie distant spécifié <OSEntryLineNum>. Ajoute une option /redirect spécifié <OSEntryLineNum> et une redirection = com<X> à la section [boot loader]. La valeur de com<X> est définie par le **portd’insertion/** paramètre.<br /><br />**DÉSACTIVER** -désactive la sortie à un ordinateur distant. Supprime l’option /redirect spécifié <OSEntryLineNum> et la redirection = com<X> définition à partir de la section [boot loader].<br /><br />**modifier** -autorise les modifications apportées aux paramètres de port en modifiant la redirection = com<X> définition dans la section [boot loader]. La valeur de com<X> est réinitialisée à la valeur spécifiée par le **portd’insertion/** paramètre.|
|/s <computer>|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u <Domain>\\<User>|Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain> \\ <User>. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande.|
|/p <Password>|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|/port {COM1 &#124; COM2 &#124; COM3 &#124; COM4 &#124; BIOSSET}|Spécifie le port COM à utiliser pour la redirection. **BIOSSET** dirige les Services de gestion d’urgence pour obtenir les paramètres du BIOS pour déterminer le port doit être utilisé pour la redirection. N’utilisez pas le **portd’insertion/** paramètre de sortie d’une administration à distance est désactivé.|
|/baud {9600 &#124; 19 200 bauds &#124; 38400&#124; 57600 &#124; 115200}|Spécifie la vitesse de transmission à utiliser pour la redirection. N’utilisez pas le **/baud** paramètre de sortie d’une administration à distance est désactivé.|
|/id <OSEntryLineNum>|Spécifie le numéro de ligne d’entrée système d’exploitation à laquelle l’option Services de gestion d’urgence est ajoutée dans la section [operating systems] du fichier Boot.ini. La première ligne après l’en-tête de section de la section [operating systems] est 1. Ce paramètre est obligatoire lorsque la valeur de Services de gestion d’urgence est définie sur **ON** ou **OFF**.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="BKMK_examples"></a>Exemples
Les exemples suivants montrent comment vous pouvez utiliser la **bootcfg /ems** commande :
```
bootcfg /ems on /port com1 /baud 19200 /id 2 
bootcfg /ems on /port biosset /id 3 
bootcfg /s srvmain /ems off /id 2 
bootcfg /ems edit /port com2 /baud 115200 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
