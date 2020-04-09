---
title: bootcfg rmsw
description: La rubrique commandes Windows pour bootcfg Rmsw, qui supprime les options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 956732f396e0fa353a8acd55953e46605a5c4200
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848442"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime les options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.

## <a name="syntax"></a>Syntaxe
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
### <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                                                      Description                                                                                                       |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                   Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                   |
| /u <Domain>\\<User>  |          Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain>\\<User>. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande.          |
|    /p <Password>     |                                                                 Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                  |
|         /mm          |           supprime l’option/MAXMEM et la valeur de mémoire maximale associée de la <OSEntryLineNum>spécifiée. L’option/MAXMEM spécifie la quantité maximale de RAM que le système d’exploitation peut utiliser.            |
|         /bv          |                     supprime l’option/basevideo du <OSEntryLineNum>spécifié. L’option/basevideo indique au système d’exploitation d’utiliser le mode VGA standard pour le pilote vidéo installé.                     |
|         /So          |                         supprime l’option/SOS du <OSEntryLineNum>spécifié. L’option/SOS indique au système d’exploitation d’afficher les noms des pilotes de périphériques pendant leur chargement.                          |
|         /ng          |                         supprime l’option/noguiboot du <OSEntryLineNum>spécifié. L’option/noguiboot désactive la barre de progression qui s’affiche avant l’invite d’ouverture de session CTRL + ALT + SUPPR.                          |
| /ID <OSEntryLineNum> | Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini à partir duquel les options de chargement du système d’exploitation sont supprimées. La première ligne après l’en-tête de la section [Operating Systems] est 1. |
|          /?          |                                                                                          Affiche l'aide à l'invite de commandes.                                                                                          |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/Rmsw**:
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
