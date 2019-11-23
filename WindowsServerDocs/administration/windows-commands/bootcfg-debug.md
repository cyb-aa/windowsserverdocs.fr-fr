---
title: bootcfg debug
description: Rubrique relative aux commandes Windows pour **bootcfg Debug** -ajoute ou modifie les paramètres de débogage pour une entrée de système d’exploitation spécifiée.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f6659cf2bfdf83b1b2fe6f6c811365775526768a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380053"
---
# <a name="bootcfg-debug"></a>bootcfg debug

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute ou modifie les paramètres de débogage pour une entrée de système d’exploitation spécifiée.

## <a name="syntax"></a>Syntaxe
```
bootcfg /debug {ON | OFF | edit}[/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Paramètres

|                           Paramètre                           |                                                                                                                                                                                                                    Description                                                                                                                                                                                                                    |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  {À &#124; l'&#124; arrêt de la modification}                   | Spécifie la valeur à déboguer.<br /><br />Activer la prise en **charge du** débogage à distance en ajoutant l’option/debug au <OSEntryLineNum>spécifié.<br /><br />**Off** -désactive la prise en charge du débogage à distance en supprimant l’option/debug de la <OSEntryLineNum>spécifiée.<br /><br />**modifier** : autorise les modifications des paramètres de port et de vitesse en bauds en modifiant les valeurs associées à l’option/debug pour la <OSEntryLineNum>spécifiée. |
|                         /s <computer>                         |                                                                                                                                                                Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                                                                                                 |
|                      /u <Domain>\\<User>                      |                                                                                                                       Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain>\\<User>. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande.                                                                                                                        |
|                         /p <Password>                         |                                                                                                                                                                               Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                                                                                                                               |
|       /port {COM1 &#124; COM2 &#124; COM3 &#124; COM4}        |                                                                                                                                                                Spécifie le port COM à utiliser pour le débogage. N’utilisez pas le paramètre **/port** si le débogage est désactivé.                                                                                                                                                                |
| /Baud {9600&#124; 19200&#124; 38400&#124; 57600&#124; 115200} |                                                                                                                                                               Spécifie la vitesse en bauds à utiliser pour le débogage. N’utilisez pas le paramètre **/Baud** si le débogage est désactivé.                                                                                                                                                                |
|                     /ID <OSEntryLineNum>                      |                                                                                                               Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini auquel les options de débogage sont ajoutées. La première ligne après l’en-tête de la section [Operating Systems] est 1.                                                                                                                |
|                              /?                               |                                                                                                                                                                                                       Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                        |

##### <a name="remarks"></a>Notes
- Si le débogage de port 1394 est requis, utilisez [bootcfg dbg1394](bootcfg-dbg1394.md).
  ## <a name="BKMK_examples"></a>Illustre
  Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/Debug**:
  ```
  bootcfg /debug on /port com1 /id 2 
  bootcfg /debug edit /port com2 /baud 19200 /id 2 
  bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
  ```
  #### <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
