---
title: bootcfg addsw
description: Rubrique de commandes de Windows pour **bootcfg addsw** -ajoute des options de chargement de système d’exploitation pour une entrée de système d’exploitation spécifié.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a056cec15bf804dafed4c4d39a80386e58c87fea
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434883"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute les options de chargement de système d’exploitation pour une entrée de système d’exploitation spécifié.

## <a name="syntax"></a>Syntaxe
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
## <a name="parameters"></a>Paramètres

|         Terme         |                                                                                                            Définition                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                        Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                        |
| /u <Domain>\\<User>  |               Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain> \\ <User>. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande.               |
|    /p <Password>     |                                                                      Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.                                                                       |
|   /mm <MaximumRAM>   |                                          Spécifie la quantité maximale de mémoire RAM, en mégaoctets, que le système d’exploitation peut utiliser. La valeur doit être égale ou supérieure à 32 mégaoctets.                                          |
|         /bv          |                                    Ajoute le **/basevideo** option spécifié <OSEntryLineNum>, diriger le système d’exploitation à utiliser le mode VGA standard pour le pilote vidéo installé.                                     |
|         /so          |                                      Ajoute le **/SOS** option spécifié *NumLigneEntréeSE*, diriger le système d’exploitation pour afficher les noms de pilote de périphérique pendant qu’ils sont chargés.                                      |
|         /ng          |                                         Ajoute le **option** option spécifié <OSEntryLineNum>, la désactivation de la barre de progression qui apparaît avant le CTRL + ALT + SUPPR d’ouverture de session invite.                                          |
| /id <OSEntryLineNum> | Spécifie le numéro de ligne d’entrée système d’exploitation dans la section [operating systems] du fichier Boot.ini auquel sont ajoutées les options de chargement du système d’exploitation. La première ligne après l’en-tête de section de la section [operating systems] est 1. |
|          /?          |                                                                                               Affiche l'aide à l'invite de commandes.                                                                                               |

## <a name="BKMK_examples"></a>Exemples
Les exemples suivants montrent comment vous pouvez utiliser la **bootcfg /addsw** commande :
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
