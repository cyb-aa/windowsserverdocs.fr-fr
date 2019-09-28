---
title: bootcfg ems
description: 'Rubrique relative aux commandes Windows pour **bootcfg EMS** : permet à l’utilisateur d’ajouter ou de modifier les paramètres de redirection de la console des services de gestion d’urgence vers un ordinateur distant.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c0e761f3796121cc71be88d23e25371369a3ee90
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379962"
---
# <a name="bootcfg-ems"></a>bootcfg ems

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Permet à l’utilisateur d’ajouter ou de modifier les paramètres de redirection de la console des services de gestion d’urgence vers un ordinateur distant. En activant les services de gestion d’urgence, vous ajoutez une ligne « Redirect = port # » à la section [Boot Loader] du fichier Boot. ini et une option/Redirect à la ligne d’entrée du système d’exploitation spécifiée. La fonctionnalité des services de gestion d’urgence est activée uniquement sur les serveurs.

## <a name="syntax"></a>Syntaxe
```
bootcfg /ems {ON | OFF | edit} [/s <computer> [/u <Domain>\<User> /p <Password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Paramètres

|                            Paramètre                             |                                                                                                                                                                                                                                                                                                                                                              Description                                                                                                                                                                                                                                                                                                                                                              |
|------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    {À &#124; l'&#124; arrêt de la modification}                    | Spécifie la valeur pour la redirection des services de gestion d’urgence.<br /><br />**Activé** -active la sortie à distance pour le @no__t spécifié-1. Ajoute une option/Redirect aux paramètres spécifiés <OSEntryLineNum> et Redirect = com @ no__t-1 à la section [Boot Loader]. La valeur de com @ no__t-0 est définie par le paramètre **/port** .<br /><br />**Désactivé** : désactive la sortie vers un ordinateur distant. supprime l’option/Redirect des @no__t spécifiées-0 et du paramètre Redirect = com @ no__t-1 de la section [Boot Loader].<br /><br />**modifier** : autorise les modifications des paramètres de port en modifiant le paramètre Redirect = com @ no__t-1 dans la section [Boot Loader]. La valeur de com @ no__t-0 est réinitialisée à la valeur spécifiée par le paramètre **/port** . |
|                          /s <computer>                           |                                                                                                                                                                                                                                                                                                          Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                                                                                                                                                                                                                                           |
|                       /u <Domain> @ no__t-1 @ no__t-2                        |                                                                                                                                                                                                                                                                 Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain> @ no__t-2 @ no__t-3. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande.                                                                                                                                                                                                                                                                  |
|                          /p <Password>                           |                                                                                                                                                                                                                                                                                                                         Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                                                                                                                                                                                                                                                                         |
| /port {COM1 &#124; COM2 &#124; COM3 &#124; COM4 &#124; BIOSSET}  |                                                                                                                                                                                                                              Spécifie le port COM à utiliser pour la redirection. **BIOSSET** dirige les services de gestion d’urgence pour obtenir les paramètres du BIOS afin de déterminer le port à utiliser pour la redirection. N’utilisez pas le paramètre **/port** si la sortie administrée à distance est désactivée.                                                                                                                                                                                                                              |
| /Baud {9600 &#124; 19200 &#124; 38400&#124; 57600 &#124; 115200} |                                                                                                                                                                                                                                                                                               Spécifie la vitesse en bauds à utiliser pour la redirection. N’utilisez pas le paramètre **/Baud** si la sortie administrée à distance est désactivée.                                                                                                                                                                                                                                                                                               |
|                       /ID <OSEntryLineNum>                       |                                                                                                                                                                                              Spécifie le numéro de ligne d’entrée du système d’exploitation dans lequel l’option services de gestion d’urgence est ajoutée dans la section [Operating Systems] du fichier Boot. ini. La première ligne après l’en-tête de la section [Operating Systems] est 1. Ce paramètre est obligatoire lorsque la valeur de l’option services de gestion d’urgence est **activée** ou **désactivée**.                                                                                                                                                                                              |
|                                /?                                |                                                                                                                                                                                                                                                                                                                                                 Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                                                                                  |

## <a name="BKMK_examples"></a>Illustre
Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/EMS** :
```
bootcfg /ems on /port com1 /baud 19200 /id 2 
bootcfg /ems on /port biosset /id 3 
bootcfg /s srvmain /ems off /id 2 
bootcfg /ems edit /port com2 /baud 115200 
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
