---
title: freedisk
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac2f38fb1948a26ae55713ac6fb14254851b062a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439161"
---
# <a name="freedisk"></a>freedisk

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie si la quantité d’espace disque spécifiée est disponible avant de poursuivre un processus d’installation.

## <a name="syntax"></a>Syntaxe
```
freedisk [/s <computer> [/u [<Domain>\]<User> [/p [<Password>]]]] [/d <Drive>] [<Value>]
```
## <a name="parameters"></a>Paramètres

|       Paramètre       |                                                                                         Description                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /s <computer>     | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. Ce paramètre s’applique à tous les fichiers et dossiers spécifiés dans la commande. |
| /u [<Domain>\\]<User> |                                            Exécute le script avec les autorisations du compte d’utilisateur spécifié. La valeur par défaut est les autorisations du système.                                            |
|    /p [<Password>]    |                                                           Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans **/u**.                                                            |
|      /d <Drive>       |                              Spécifie le lecteur pour lequel vous souhaitez déterminer la disponibilité d’espace libre. Vous devez spécifier <Drive>pour un ordinateur distant.                               |
|        <Value>        |                                     Vérifie qu’une quantité spécifique d’espace disque libre. Vous pouvez spécifier <Value>en octets, Ko, Mo, Go, To, po, EB, ZB ou YB.                                      |

## <a name="remarks"></a>Notes
- À l’aide de la **/s**, **/u**, et **/p** les options de ligne de commande sont disponibles uniquement lorsque vous utilisez **/s**. Vous devez utiliser **/p** avec **/u**pour fournir le mot de passe utilisateur s.
- pour les installations sans assistance, vous pouvez utiliser **freedisk** dans les fichiers de commandes d’installation pour vérifier l’espace libre du volume requis avant de poursuivre l’installation.
- Lorsque vous utilisez **freedisk** dans un fichier de commandes, elle retourne un **0** si l’espace est suffisant et **1** si n’est pas suffisamment d’espace.
  ## <a name="BKMK_examples"></a>Exemples
  Pour déterminer s’il existe au moins 50 Mo d’espace libre disponible sur le lecteur C:, tapez :
  ```
  freedisk 50mb 
  ```
  Sortie similaire à ce qui suit s’affiche sur l’écran :
  ```
  INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
