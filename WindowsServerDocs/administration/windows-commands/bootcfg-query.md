---
title: bootcfg query
description: La rubrique commandes Windows pour **bootcfg Query** -Queries et affiche les entrées de la section [Boot Loader] et [Operating Systems] à partir de boot. ini.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae82357cfe178343872448c2ebd46c49a797b5a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379909"
---
# <a name="bootcfg-query"></a>bootcfg query

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interroge et affiche les entrées de la section [Boot Loader] et [Operating Systems] à partir de boot. ini.

## <a name="syntax"></a>Syntaxe
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
## <a name="parameters"></a>Paramètres

|        Terme         |                                                                                             Définition                                                                                              |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>    |                                         Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                          |
| /u <Domain>\\<User> | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User>ou <Domain>\\<User>. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
|    /p <Password>    |                                                        Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                        |
|         /?          |                                                                                Affiche l'aide à l'invite de commandes.                                                                                 |

##### <a name="remarks"></a>Notes
- Voici un exemple de sortie de **bootcfg/Query** :
  ```
  Boot Loader Settings
  ----------
  timeout: 30
  default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  Boot Entries
  ------
  Boot entry ID:   1
  Friendly Name:   ""
  path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  OS Load Options: /fastdetect /debug /debugport=com1:
  ```
- La partie paramètres du chargeur de démarrage de la sortie de **requête bootcfg** affiche chaque entrée dans la section [Boot Loader] de boot. ini.
- La partie entrées de démarrage de la sortie de **requête bootcfg** affiche les détails suivants pour chaque entrée du système d’exploitation dans la section [Operating Systems] de boot. ini : ID d’entrée de démarrage, nom convivial, chemin d’accès et options de chargement du système d’exploitation.
  ## <a name="BKMK_examples"></a>Illustre
  Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/Query** :
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  #### <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
