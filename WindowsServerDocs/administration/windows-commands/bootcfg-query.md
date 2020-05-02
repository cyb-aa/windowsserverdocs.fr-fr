---
title: bootcfg query
description: Rubrique de référence pour la commande bootcfg Query, qui interroge et affiche les entrées de la section du chargeur et du système d’exploitation à partir de boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44c5ce60f55618d16e80d1f1efde3af1cbf6c609
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709332"
---
# <a name="bootcfg-query"></a>bootcfg query

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interroge et affiche les entrées de la section [Boot Loader] et [Operating Systems] à partir de boot. ini.

## <a name="syntax"></a>Syntaxe

```
bootcfg /query [/s <computer> [/u <domain>\<user> /p <password>]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `/s <computer>` | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. |
| `/u <domain>\<user>`  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié `<user>` par `<domain>\<user>`ou. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
| `/p <password>` | Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="sample-output"></a>Exemple de sortie

Exemple de sortie pour la commande **bootcfg/Query** :
  
```
Boot Loader Settings
----------
timeout: 30
default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
Boot Entries
------
Boot entry ID:   1
Friendly Name:
path: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
OS Load Options: /fastdetect /debug /debugport=com1:
```

- La zone **paramètres du chargeur de démarrage** affiche chaque entrée dans la section [Boot Loader] de boot. ini.

- La zone **entrées de démarrage** affiche plus de détails pour chaque entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini.

## <a name="examples"></a>Exemples

Pour utiliser la commande **bootcfg/Query** :

```
bootcfg /query
bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
bootcfg /query /u hiropln /p p@ssW23
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande bootcfg](bootcfg.md)
