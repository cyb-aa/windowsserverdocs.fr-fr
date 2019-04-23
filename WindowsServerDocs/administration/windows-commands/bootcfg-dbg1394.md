---
title: bootcfg dbg1394
description: Rubrique de commandes de Windows pour **bootcfg dbg1394** -1394 configure port de débogage pour l’entrée spécifiée du système d’exploitation
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b36d22cea5b7b0c0e1768736d6c80c67b3c733f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857650"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure le débogage de port 1394 pour une entrée de système d’exploitation spécifié.

## <a name="syntax"></a>Syntaxe
```
bootcfg /dbg1394 {ON | OFF}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/ch <Channel>] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|{ON &#124; OFF}|Spécifie la valeur pour le débogage de port 1394.<br /><br />-   **ON** -permet la prise en charge du débogage à distance en ajoutant l’option/dbg1394 spécifié <OSEntryLineNum>.<br />-   **DÉSACTIVER** -désactive la prise en charge du débogage à distance en supprimant l’option/dbg1394 à partir du spécifié <OSEntryLineNum>.|
|/s <computer>|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u <Domain>\\<User>|Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain> \\ <User>. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande.|
|/p <Password>|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|/CH canal|Spécifie le canal à utiliser pour le débogage. Les valeurs valides sont des entiers compris entre 1 et 64. N’utilisez pas le **/ch** <Channel> paramètre si le débogage de port 1394.|
|/id <OSEntryLineNum>|Spécifie le numéro de ligne d’entrée système d’exploitation dans la section [operating systems] du fichier Boot.ini auquel sont ajoutées les options de débogage de port 1394. La première ligne après l’en-tête de section de la section [operating systems] est 1.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="BKMK_examples"></a>Exemples
Les exemples suivants montrent comment vous pouvez utiliser la **bootcfg/dbg1394**commande :
```
bootcfg /dbg1394 /id 2 
bootcfg /dbg1394 on /ch 1 /id 3 
bootcfg /dbg1394 edit /ch 8 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
