---
title: bootcfg delete
description: La rubrique commandes Windows pour bootcfg Delete, qui supprime une entrée de système d’exploitation dans la section systèmes d’exploitation du fichier Boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 71382e29-9b39-41c8-9c23-cf0ff829440a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01ec7a4dde1e22982f2cf0fa30245c33e09cc0ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848542"
---
# <a name="bootcfg-delete"></a>bootcfg delete

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime une entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini.

## <a name="syntax"></a>Syntaxe
```
bootcfg /delete [/s <computer> [/u <Domain>\<User> /p <Password>]] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>Paramètres

|         Terme         |                                                                                             Définition                                                                                              |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                          |
| /u <Domain>\\<User>  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User>ou <Domain>\\<User>. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
|    /p <Password>     |                                                        Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                        |
| /ID <OSEntryLineNum> |        Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini à supprimer. La première ligne après l’en-tête de la section [Operating Systems] est 1.        |
|          /?          |                                                                                Affiche l'aide à l'invite de commandes.                                                                                 |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/Delete**:
```
bootcfg /delete /id 1
bootcfg /delete /s srvmain /u maindom\hiropln /p p@ssW23 /id 3
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
