---
title: bootcfg delete
description: La rubrique commandes Windows pour **bootcfg Delete** -supprime une entrée du système d’exploitation dans la section systèmes d’exploitation du fichier Boot. ini.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d2298c07af32e66a2ffcebb85ec780da762be58
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380011"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

supprime une entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini.

## <a name="syntax"></a>Syntaxe
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Paramètres

|         Terme         |                                                                                             Définition                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                          |
| /u <Domain> @ no__t-1 @ no__t-2  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User>or <Domain> @ no__t-2 @ no__t-3. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
|    /p <Password>     |                                                        Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                        |
| /ID <OSEntryLineNum> |        Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini à supprimer. La première ligne après l’en-tête de la section [Operating Systems] est 1.        |
|          /?          |                                                                                Affiche l'aide à l'invite de commandes.                                                                                 |

## <a name="BKMK_examples"></a>Illustre
Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/Delete**:
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
