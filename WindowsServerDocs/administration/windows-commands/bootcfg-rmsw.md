---
title: bootcfg rmsw
description: La rubrique commandes Windows pour **bootcfg Rmsw** -supprime les options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43629f2e13bb6269a43d592fa0907637135aea71
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379856"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

supprime les options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.

## <a name="syntax"></a>Syntaxe
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                                                      Description                                                                                                       |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                   Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                   |
| /u <Domain> @ no__t-1 @ no__t-2  |          Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain> @ no__t-2 @ no__t-3. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande.          |
|    /p <Password>     |                                                                 Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                  |
|         /mm          |           supprime l’option/MAXMEM et la valeur de mémoire maximale associée du @no__t spécifié-0. L’option/MAXMEM spécifie la quantité maximale de RAM que le système d’exploitation peut utiliser.            |
|         /bv          |                     supprime l’option/basevideo du @no__t spécifié. L’option/basevideo indique au système d’exploitation d’utiliser le mode VGA standard pour le pilote vidéo installé.                     |
|         /So          |                         supprime l’option/SOS du @no__t spécifié-0. L’option/SOS indique au système d’exploitation d’afficher les noms des pilotes de périphériques pendant leur chargement.                          |
|         /ng          |                         supprime l’option/noguiboot du @no__t spécifié-0. L’option/noguiboot désactive la barre de progression qui s’affiche avant l’invite d’ouverture de session CTRL + ALT + SUPPR.                          |
| /ID <OSEntryLineNum> | Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini à partir duquel les options de chargement du système d’exploitation sont supprimées. La première ligne après l’en-tête de la section [Operating Systems] est 1. |
|          /?          |                                                                                          Affiche l'aide à l'invite de commandes.                                                                                          |

## <a name="BKMK_examples"></a>Illustre
Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/Rmsw**:
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
