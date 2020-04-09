---
title: getmac
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b593bf61bb08d2c1c7868b1bbb175ed64a8bcf7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842612"
---
# <a name="getmac"></a>getmac

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Retourne l’adresse MAC (Media Access Control) et la liste des protocoles réseau associés à chaque adresse pour toutes les cartes réseau de chaque ordinateur, localement ou sur un réseau. 
## <a name="syntax"></a>Syntaxe
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
#### <a name="parameters"></a>Paramètres

|             Paramètre              |                                                                                          Description                                                                                          |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s <computer>            |                                      Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                       |
|        /u <Domain>\\<User>         | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par user ou domaine\utilisateur. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
|           /p <Password>            |                                                     Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                     |
| /FO {TABLE &#124; List&#124; CSV} |                       Spécifie le format à utiliser pour la sortie de la requête. Les valeurs valides sont **table**, **List**et **CSV**. Le format par défaut de la sortie est **table**.                        |
|                /NH                 |                                             Supprime l’en-tête de colonne dans la sortie. Valide lorsque le paramètre **/FO** est défini sur **table** ou **CSV**.                                              |
|                 /v                 |                                                                    Spécifie que la sortie affiche des informations détaillées.                                                                     |
|                 /?                 |                                                                                                                                                                                               |

## <a name="remarks"></a>Notes
**GETMAC** peut être utile lorsque vous souhaitez entrer l’adresse Mac dans un analyseur réseau ou lorsque vous avez besoin de savoir quels protocoles sont en cours d’utilisation sur chaque carte réseau d’un ordinateur.
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **GETMAC** :
```
getmac /fo table /nh /v
```
```
getmac /s srvmain
```
```
getmac /s srvmain /u maindom\hiropln
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo list /v
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo table /nh
```
## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
