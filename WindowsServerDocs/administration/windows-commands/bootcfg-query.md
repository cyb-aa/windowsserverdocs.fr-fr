---
title: bootcfg query
description: Rubrique relative aux commandes Windows pour la requête bootcfg, qui interroge et affiche les entrées de la section chargeur de démarrage et système d’exploitation à partir de boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ac80c802b1d30dcf7221f94f761233c6b6fc6b6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848522"
---
# <a name="bootcfg-query"></a>bootcfg query

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interroge et affiche les entrées de la section [Boot Loader] et [Operating Systems] à partir de boot. ini.

## <a name="syntax"></a>Syntaxe
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
### <a name="parameters"></a>Paramètres

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
  Friendly Name:   
  path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
  OS Load Options: /fastdetect /debug /debugport=com1:
  ```
- La partie paramètres du chargeur de démarrage de la sortie de **requête bootcfg** affiche chaque entrée dans la section [Boot Loader] de boot. ini.
- La partie entrées de démarrage de la sortie de **requête bootcfg** affiche les détails suivants pour chaque entrée du système d’exploitation dans la section [Operating Systems] de boot. ini : ID d’entrée de démarrage, nom convivial, chemin d’accès et options de chargement du système d’exploitation.
  ## <a name="examples"></a><a name=BKMK_examples></a>Illustre
  Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/Query** :
  ```
  bootcfg /query
  bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
  bootcfg /query /u hiropln /p p@ssW23
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
