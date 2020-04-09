---
title: bootcfg dbg1394
description: Rubrique commandes Windows pour bootcfg dbg1394, qui configure le débogage de port 1394 pour une entrée de système d’exploitation spécifiée
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d49e0a39cd021f093ca68bf97963dc35c3b53ad4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848672"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure le débogage de port 1394 pour une entrée de système d’exploitation spécifiée.

## <a name="syntax"></a>Syntaxe
```
bootcfg /dbg1394 {ON | OFF}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/ch <Channel>] /id <OSEntryLineNum>
```
### <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                                                                                           Description                                                                                                                                            |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   {ON &#124; OFF}    | Spécifie la valeur du débogage du port 1394.<p>-   **active** la prise en charge du débogage à distance en ajoutant l’option/dbg1394 au <OSEntryLineNum>spécifié.<br />-   **désactivé** -désactive la prise en charge du débogage à distance en supprimant l’option/dbg1394 de la <OSEntryLineNum>spécifiée. |
|    /s <computer>     |                                                                                        Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                        |
| /u <Domain>\\<User>  |                                               Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain>\\<User>. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande.                                               |
|    /p <Password>     |                                                                                                      Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                                                       |
|     /ch canal      |                                                           Spécifie le canal à utiliser pour le débogage. Les valeurs valides sont des entiers compris entre 1 et 64. N’utilisez pas le paramètre **/ch** <Channel> si le débogage de port 1394 est désactivé.                                                           |
| /ID <OSEntryLineNum> |                                  Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini auquel sont ajoutées les options de débogage du port 1394. La première ligne après l’en-tête de la section [Operating Systems] est 1.                                  |
|          /?          |                                                                                                                               Affiche l'aide à l'invite de commandes.                                                                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/dbg1394**:
```
bootcfg /dbg1394 /id 2 
bootcfg /dbg1394 on /ch 1 /id 3 
bootcfg /dbg1394 edit /ch 8 /id 2 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
