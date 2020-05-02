---
title: bootcfg timeout
description: Rubrique de référence pour la commande bootcfg Timeout, qui modifie la valeur du délai d’attente du système d’exploitation.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e10ccab69ca58a6cef260546e965f90eecfc8beb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708942"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie la valeur du délai d’attente du système d’exploitation.

## <a name="syntax"></a>Syntaxe

```
bootcfg /timeout <timeoutvalue> [/s <computer> [/u <domain>\<user> /p <password>]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `/timeout <timeoutvalue>` | Spécifie la valeur du délai d’attente dans la section [Boot Loader]. Le `<timeoutvalue>` est le nombre de secondes dont dispose l’utilisateur pour sélectionner un système d’exploitation à partir de l’écran de chargement de démarrage avant que ntldr ne charge la valeur par défaut. La plage valide pour `<timeoutvalue>` est 0-999. Si la valeur est 0, NTLDR démarre immédiatement le système d’exploitation par défaut sans afficher l’écran du chargeur de démarrage. |
| `/s <computer>` | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. |
| `/u <domain>\<user>`  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié `<user>` par `<domain>\<user>`ou. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
| `/p <password>` | Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour utiliser la commande **bootcfg/Timeout** :

```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande bootcfg](bootcfg.md)
