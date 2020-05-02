---
title: bootcfg debug
description: Rubrique de référence pour la commande bootcfg Debug, qui ajoute ou modifie les paramètres de débogage pour une entrée de système d’exploitation spécifiée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 28afa5fb-a236-46e2-b1a4-a3c43a49c437
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8059aaefd1b23b3e74f4c27ba96e322c44b5cb6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709710"
---
# <a name="bootcfg-debug"></a>bootcfg debug

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute ou modifie les paramètres de débogage pour une entrée de système d’exploitation spécifiée.

>[!NOTE]
> Si vous essayez de déboguer le port 1394, utilisez la commande [bootcfg dbg1394](bootcfg-dbg1394.md) à la place.

## <a name="syntax"></a>Syntaxe

```
bootcfg /debug {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `{on | off | edit}` | Spécifie la valeur pour le débogage de port, notamment :<ul><li>**sur.** Active la prise en charge du débogage à distance en ajoutant l’option `<osentrylinenum>`/Debug au spécifié.</li><li>**préférable.** Désactive la prise en charge du débogage à distance en supprimant l’option/debug <osentrylinenum>du spécifié.</li><li>**modifiés.** Autorise les modifications des paramètres de port et de vitesse en bauds en modifiant les valeurs associées à l’option <osentrylinenum>/Debug pour le spécifié.</li></ul> |
| `/s <computer>` | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. |
| `/u <domain>\<user>`  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié `<user>` par `<domain>\<user>`ou. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
| `/p <password>` | Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . |
| `/port {COM1 | COM2 | COM3 | COM4}` |  Spécifie le port COM à utiliser pour le débogage. N’utilisez pas ce paramètre si le débogage est désactivé. |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | Spécifie la vitesse en bauds à utiliser pour le débogage. N’utilisez pas ce paramètre si le débogage est désactivé. |
| `/id <osentrylinenum>` | Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini auquel les options de chargement du système d’exploitation sont ajoutées. La première ligne après l’en-tête de la section [Operating Systems] est 1. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour utiliser la commande **bootcfg/Debug** :

```
bootcfg /debug on /port com1 /id 2
bootcfg /debug edit /port com2 /baud 19200 /id 2
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /debug off /id 2
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande bootcfg](bootcfg.md)
