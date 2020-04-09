---
title: bootcfg copy
description: La rubrique commandes Windows pour bootcfg Copy, qui effectue une copie d’une entrée de démarrage existante, à laquelle vous pouvez ajouter des options de ligne de commande.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5194418a07aece4f15a84c3eccbc044431a865b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848682"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Effectue une copie d’une entrée de démarrage existante, à laquelle vous pouvez ajouter des options de ligne de commande.

## <a name="syntax"></a>Syntaxe
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                                             Description                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                          |
| /u <Domain>\\<User>  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User>ou <Domain>\\<User>. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
|    /p <Password>     |                                                        Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                        |
|   /d <Description>   |                                                                    Spécifie la description de la nouvelle entrée du système d’exploitation.                                                                    |
| /ID <OSEntryLineNum> |         Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini à copier. La première ligne après l’en-tête de la section [Operating Systems] est 1.         |
|          /?          |                                                                                Affiche l'aide à l'invite de commandes.                                                                                 |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Les exemples suivants montrent comment vous pouvez utiliser la commande **bootcfg/Copy** pour copier l’entrée de démarrage 1 et entrer \ABC Server\\ comme Description :
```
bootcfg /copy /d \ABC Server\ /id 1
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
