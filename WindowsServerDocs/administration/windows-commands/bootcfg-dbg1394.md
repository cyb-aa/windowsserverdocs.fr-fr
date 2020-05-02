---
title: bootcfg dbg1394
description: Rubrique de référence pour la commande bootcfg dbg1394, qui configure le débogage de port 1394 pour une entrée de système d’exploitation spécifiée
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 35724697-90dd-4dbe-85b0-337fbd369dcc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16230c52657fd5c9c14972726ed2465401995223
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709704"
---
# <a name="bootcfg-dbg1394"></a>bootcfg dbg1394

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure le débogage de port 1394 pour une entrée de système d’exploitation spécifiée.

## <a name="syntax"></a>Syntaxe

```
bootcfg /dbg1394 {on | off}[/s <computer> [/u <domain>\<user> /p <password>]] [/ch <channel>] /id <osentrylinenum>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `{on | off}` | Spécifie la valeur du débogage du port 1394, y compris :<ul><li>**sur.** Active la prise en charge du débogage à distance en ajoutant l’option `<osentrylinenum>`/dbg1394 au spécifié.</li><li>**préférable.** Désactive la prise en charge du débogage à distance en supprimant l’option/dbg1394 <osentrylinenum>du spécifié.</li></ul> |
| `/s <computer>` | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. |
| `/u <domain>\<user>`  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié `<user>` par `<domain>\<user>`ou. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
| `/p <password>` | Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . |
| `/ch <channel>` | Spécifie le canal à utiliser pour le débogage. Les valeurs valides incluent des entiers compris entre 1 et 64. N’utilisez pas ce paramètre si le débogage de port 1394 est désactivé. |
| `/id <osentrylinenum>` | Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini auquel les options de chargement du système d’exploitation sont ajoutées. La première ligne après l’en-tête de la section [Operating Systems] est 1. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour utiliser la commande **bootcfg/dbg1394**:

```
bootcfg /dbg1394 /id 2
bootcfg /dbg1394 on /ch 1 /id 3
bootcfg /dbg1394 edit /ch 8 /id 2
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /dbg1394 off /id 2
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande bootcfg](bootcfg.md)
