---
title: bootcfg ems
description: Rubrique de référence pour la commande bootcfg EMS, qui permet à l’utilisateur d’ajouter ou de modifier les paramètres de redirection de la console des services de gestion d’urgence vers un ordinateur distant.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57abdc50-c64a-45f1-8470-3f8c3a51f743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c85231e094852feb673eb4f99b183f06014234b2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709524"
---
# <a name="bootcfg-ems"></a>bootcfg ems

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permet à l’utilisateur d’ajouter ou de modifier les paramètres de redirection de la console des services de gestion d’urgence vers un ordinateur distant. L’activation des services de gestion d' `redirect=Port#` urgence, ajoute une ligne à la section [Boot Loader] du fichier Boot. ini avec l’option/Redirect à la ligne d’entrée du système d’exploitation spécifiée. La fonctionnalité des services de gestion d’urgence est activée uniquement sur les serveurs.

## <a name="syntax"></a>Syntaxe

```
bootcfg /ems {on | off | edit}[/s <computer> [/u <domain>\<user> /p <password>]] [/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}] [/baud {9600 | 19200 | 38400 | 57600 | 115200}] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `{on | off | edit}` | Spécifie la valeur pour la redirection des services de gestion d’urgence, notamment :<ul><li>**sur.** Active la sortie à distance pour `<osentrylinenum>`le spécifié. Ajoute également une option/Redirect au spécifié <osentrylinenum> et un `redirect=com<X>` paramètre à la section [Boot Loader]. La valeur de `com<X>` est définie par le paramètre **/port** .</li><li>**préférable.** Désactive la sortie vers un ordinateur distant. Supprime également l’option/Redirect du spécifié <osentrylinenum> et le `redirect=com<X>` paramètre de la section [Boot Loader].</li><li>**modifiés.** Autorise les modifications des paramètres de port en `redirect=com<X>` modifiant le paramètre dans la section [Boot Loader]. La valeur de `com<X>` est définie par le paramètre **/port** .</li></ul> |
| `/s <computer>` | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. |
| `/u <domain>\<user>`  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié `<user>` par `<domain>\<user>`ou. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
| `/p <password>` | Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . |
| `/port {COM1 | COM2 | COM3 | COM4 | BIOSSET}` |  Spécifie le port COM à utiliser pour la redirection. Le paramètre BIOSSET dirige les services de gestion d’urgence pour obtenir les paramètres du BIOS afin de déterminer le port à utiliser pour la redirection. N’utilisez pas ce paramètre si la sortie administrée à distance est désactivée. |
| `/baud {9600 | 19200 | 38400 | 57600 | 115200}` | Spécifie la vitesse en bauds à utiliser pour la redirection. N’utilisez pas ce paramètre si la sortie administrée à distance est désactivée. |
| `/id <osentrylinenum>` | Spécifie le numéro de ligne d’entrée du système d’exploitation dans lequel l’option services de gestion d’urgence est ajoutée dans la section [Operating Systems] du fichier Boot. ini. La première ligne après l’en-tête de la section [Operating Systems] est 1. Ce paramètre est obligatoire lorsque la valeur de l’option services de gestion d’urgence est **activée** ou **désactivée**. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour utiliser la commande **bootcfg/EMS** :

```
bootcfg /ems on /port com1 /baud 19200 /id 2
bootcfg /ems on /port biosset /id 3
bootcfg /s srvmain /ems off /id 2
bootcfg /ems edit /port com2 /baud 115200
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /ems off /id 2
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande bootcfg](bootcfg.md)
