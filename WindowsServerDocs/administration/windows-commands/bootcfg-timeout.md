---
title: bootcfg timeout
description: Rubrique Windows Commands for **bootcfg Timeout** -modifie la valeur du délai d’attente du système d’exploitation.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94bc2de43dd179117c7a44747961213d12741a09
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379868"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

modifie la valeur du délai d’attente du système d’exploitation.

## <a name="syntax"></a>Syntaxe
```
bootcfg /timeout <timeOutValue> [/s <computer> [/u <Domain\User>/p <Password>]]
```
## <a name="parameters"></a>Paramètres

|        Paramètre        |                                                                                                                                                                                  Description                                                                                                                                                                                   |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Timeout <timeOutValue> | Spécifie la valeur du délai d’attente dans la section [Boot Loader]. La <timeOutValue> représente le nombre de secondes pendant lesquelles l’utilisateur doit sélectionner un système d’exploitation à partir de l’écran de chargement de démarrage avant que NTLDR ne charge la valeur par défaut. Plage valide pour <timeOutValue> est 0-999. Si la valeur est 0, NTLDR démarre immédiatement le système d’exploitation par défaut sans afficher l’écran du chargeur de démarrage. |
|      /s <computer>      |                                                                                                                               Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                                                               |
|    /u < domaine\utilisateur >     |                                                                                       Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou < domaine\utilisateur >. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande.                                                                                        |
|      /p <Password>      |                                                                                                                                            Spécifie le <Password> du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                                                                                             |
|           /?            |                                                                                                                                                                      Affiche l'aide à l'invite de commandes.                                                                                                                                                                      |

## <a name="BKMK_examples"></a>Illustre
Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/Timeout** :
```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
