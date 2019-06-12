---
title: bootcfg rmsw
description: Rubrique de commandes de Windows pour **bootcfg rmsw** - supprime d’exploitation des options de chargement pour une entrée de système d’exploitation spécifié.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f3d873cffbdb386b5f4f564801a4f4b815c6987a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434678"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime les options de chargement de système d’exploitation pour une entrée de système d’exploitation spécifié.

## <a name="syntax"></a>Syntaxe
```
bootcfg /rmsw [/s <computer> [/u <Domain>\<User> [/p <Password>]]] [/mm] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                                                      Description                                                                                                       |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                   Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                   |
| /u <Domain>\\<User>  |          Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain> \\ <User>. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande.          |
|    /p <Password>     |                                                                 Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.                                                                  |
|         /mm          |           Supprime l’option/maxmem et sa valeur de mémoire maximale associée à partir du spécifié <OSEntryLineNum>. L’option/maxmem spécifie la quantité maximale de mémoire RAM que le système d’exploitation peut utiliser.            |
|         /bv          |                     Supprime l’option /basevideo spécifié <OSEntryLineNum>. L’option /basevideo indique le système d’exploitation à utiliser le mode VGA standard pour le pilote vidéo installé.                     |
|         /so          |                         Supprime l’option/sos spécifié <OSEntryLineNum>. L’option/sos dirige le système d’exploitation pour afficher les noms de pilote de périphérique pendant qu’ils sont chargés.                          |
|         /ng          |                         Supprime l’option d’option spécifié <OSEntryLineNum>. L’option de l’option désactive la barre de progression qui apparaît avant le CTRL + ALT + SUPPR invite d’ouverture.                          |
| /id <OSEntryLineNum> | Spécifie le numéro de ligne d’entrée système d’exploitation dans la section [operating systems] du fichier Boot.ini à partir de laquelle les Options de chargement du système d’exploitation sont supprimées. La première ligne après l’en-tête de section de la section [operating systems] est 1. |
|          /?          |                                                                                          Affiche l'aide à l'invite de commandes.                                                                                          |

## <a name="BKMK_examples"></a>Exemples
Les exemples suivants montrent comment vous pouvez utiliser la **bootcfg /rmsw**commande :
```
bootcfg /rmsw /mm 64 /id 2 
bootcfg /rmsw /so /id 3 
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /rmsw /ng /id 2 
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2       
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
