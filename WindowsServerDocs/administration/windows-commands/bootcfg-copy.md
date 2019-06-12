---
title: bootcfg copy
description: Rubrique de commandes de Windows pour **bootcfg copie** -effectue une copie d’une entrée de démarrage existante, à laquelle vous pouvez ajouter des options de ligne de commande.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b76ecfe953d1a462e311fdaaeba35e8f962165c4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434860"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Effectue une copie d’une entrée de démarrage existante, auquel vous pouvez ajouter des options de ligne de commande.

## <a name="syntax"></a>Syntaxe
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                                             Description                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                          |
| /u <Domain>\\<User>  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User>ou <Domain> \\ <User>. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande. |
|    /p <Password>     |                                                        Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.                                                        |
|   /d <Description>   |                                                                    Spécifie la description de la nouvelle entrée de système d’exploitation.                                                                    |
| /id <OSEntryLineNum> |         Spécifie le numéro de ligne d’entrée système d’exploitation dans la section [operating systems] du fichier Boot.ini à copier. La première ligne après l’en-tête de section de la section [operating systems] est 1.         |
|          /?          |                                                                                Affiche l'aide à l'invite de commandes.                                                                                 |

## <a name="BKMK_examples"></a>Exemples
Les exemples suivants montrent comment vous pouvez utiliser la **bootcfg /copy** commande pour copier l’entrée de démarrage 1 et entrez « \ABC Server\\» comme description :
```
bootcfg /copy /d "\ABC Server\" /id 1
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
