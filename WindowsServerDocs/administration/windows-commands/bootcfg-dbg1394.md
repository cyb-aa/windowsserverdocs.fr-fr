---
title: bootcfg dbg1394
description: La rubrique commandes Windows pour **bootcfg dbg1394** -configure le débogage de port 1394 pour une entrée de système d’exploitation spécifiée
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8550871c60343fdc6d797f3f81729c24270400b4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380089"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Configure le débogage de port 1394 pour une entrée de système d’exploitation spécifiée.

## <a name="syntax"></a>Syntaxe
```
bootcfg /dbg1394 {ON | OFF}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/ch <Channel>] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                                                                                           Description                                                                                                                                            |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   {ON &#124; OFF}    | Spécifie la valeur du débogage du port 1394.<br /><br />@no__t **-0 active** -active la prise en charge du débogage à distance en ajoutant l’option/dbg1394 à la <OSEntryLineNum> spécifiée.<br />-   **off** -désactive la prise en charge du débogage à distance en supprimant l’option/dbg1394 de l' <OSEntryLineNum> spécifiée. |
|    /s <computer>     |                                                                                        Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                        |
| /u <Domain> @ no__t-1 @ no__t-2  |                                               Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain> @ no__t-2 @ no__t-3. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande.                                               |
|    /p <Password>     |                                                                                                      Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                                                       |
|     /ch canal      |                                                           Spécifie le canal à utiliser pour le débogage. Les valeurs valides sont des entiers compris entre 1 et 64. N’utilisez pas le paramètre **/ch** <Channel> si le débogage de port 1394 est désactivé.                                                           |
| /ID <OSEntryLineNum> |                                  Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini auquel sont ajoutées les options de débogage du port 1394. La première ligne après l’en-tête de la section [Operating Systems] est 1.                                  |
|          /?          |                                                                                                                               Affiche l'aide à l'invite de commandes.                                                                                                                               |

## <a name="BKMK_examples"></a>Illustre
Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/dbg1394**:
```
bootcfg /dbg1394 /id 2 
bootcfg /dbg1394 on /ch 1 /id 3 
bootcfg /dbg1394 edit /ch 8 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
