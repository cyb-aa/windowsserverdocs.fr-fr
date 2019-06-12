---
title: bootcfg delete
description: Rubrique de commandes de Windows pour **bootcfg supprimer** -supprime une entrée de système d’exploitation dans la section systèmes d’exploitation du fichier Boot.ini.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 37f30a181402fe8a74148b42398641af3c4846b9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434765"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime une entrée de système d’exploitation dans la section [operating systems] du fichier Boot.ini.

## <a name="syntax"></a>Syntaxe
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Paramètres

|         Terme         |                                                                                             Définition                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                          |
| /u <Domain>\\<User>  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User>ou <Domain> \\ <User>. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande. |
|    /p <Password>     |                                                        Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.                                                        |
| /id <OSEntryLineNum> |        Spécifie le numéro de ligne d’entrée système d’exploitation dans la section [operating systems] du fichier Boot.ini à supprimer. La première ligne après l’en-tête de section de la section [operating systems] est 1.        |
|          /?          |                                                                                Affiche l'aide à l'invite de commandes.                                                                                 |

## <a name="BKMK_examples"></a>Exemples
Les exemples suivants montrent comment vous pouvez utiliser la **bootcfg /delete**commande :
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
