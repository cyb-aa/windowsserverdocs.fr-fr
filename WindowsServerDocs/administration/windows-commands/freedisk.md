---
title: freedisk
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 102d829535eb6028b2a062234f280fd32ea48943
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725555"
---
# <a name="freedisk"></a>freedisk

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie si la quantité d’espace disque spécifiée est disponible avant de poursuivre un processus d’installation.

## <a name="syntax"></a>Syntaxe
```
freedisk [/s <computer> [/u [<Domain>\]<User> [/p [<Password>]]]] [/d <Drive>] [<Value>]
```
### <a name="parameters"></a>Paramètres

|       Paramètre       |                                                                                         Description                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     commutateur<computer>     | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. Ce paramètre s’applique à tous les fichiers et dossiers spécifiés dans la commande. |
| /u [<Domain>\\]<User> |                                            Exécute le script avec les autorisations du compte d’utilisateur spécifié. La valeur par défaut est autorisations système.                                            |
|    /p [<Password>]    |                                                           Spécifie le mot de passe du compte d’utilisateur spécifié dans **/u**.                                                            |
|      /d<Drive>       |                              Spécifie le lecteur pour lequel vous souhaitez déterminer la disponibilité de l’espace libre. Vous devez spécifier <Drive>pour un ordinateur distant.                               |
|        <Value>        |                                     Vérifie la quantité d’espace disque disponible. Vous pouvez spécifier : <Value>en octets, Ko, Mo, Go, to, PB, EB, ZB ou YB.                                      |

## <a name="remarks"></a>Notes 
- L’utilisation des options de ligne de commande **/s**, **/u**et **/p** est disponible uniquement lorsque vous utilisez **/s**. Vous devez utiliser **/p** avec **/u**pour fournir le mot de passe utilisateur.
- pour les installations sans assistance, vous pouvez utiliser **freedisk** dans les fichiers de commandes d’installation pour vérifier la quantité d’espace libre requise avant de poursuivre l’installation.
- Quand vous utilisez **freedisk** dans un fichier de commandes, elle retourne **0** s’il y a suffisamment d’espace et **1** si l’espace est insuffisant.
  ## <a name="examples"></a>Exemples
  Pour déterminer si au moins 50 Mo d’espace libre sont disponibles sur le lecteur C :, tapez :
  ```
  freedisk 50mb 
  ```
  Une sortie similaire à ce qui suit s’affiche à l’écran :
  ```
  INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
