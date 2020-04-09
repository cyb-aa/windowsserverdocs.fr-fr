---
title: bootcfg addsw
description: La rubrique commandes Windows pour bootcfg Addsw, qui ajoute des options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d8389293-ecd9-42f0-b84b-b9ead4cf52e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9ae5175dfc3b068276f6ab95d6823699c96b2b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848712"
---
# <a name="bootcfg-addsw"></a>bootcfg addsw

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute des options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.

## <a name="syntax"></a>Syntaxe
```
bootcfg /addsw [/s <computer> [/u <Domain>\<User> /p <Password>]] [/mm <MaximumRAM>] [/bv] [/so] [/ng] /id <OSEntryLineNum>
```
### <a name="parameters"></a>Paramètres

|         Terme         |                                                                                                            Définition                                                                                                            |
|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                                        Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                        |
| /u <Domain>\\<User>  |               Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou <Domain>\\<User>. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande.               |
|    /p <Password>     |                                                                      Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                       |
|   <MaximumRAM>/mm   |                                          Spécifie la quantité maximale de mémoire vive (en mégaoctets) pouvant être utilisée par le système d’exploitation. La valeur doit être supérieure ou égale à 32 mégaoctets.                                          |
|         /bv          |                                    Ajoute l’option **/basevideo** au <OSEntryLineNum>spécifié, en dirigeant le système d’exploitation pour qu’il utilise le mode VGA standard pour le pilote vidéo installé.                                     |
|         /So          |                                      Ajoute l’option **/SOS** à la valeur *NumLigneEntréeSE*spécifiée, en dirigeant le système d’exploitation afin d’afficher les noms des pilotes de périphériques pendant leur chargement.                                      |
|         /ng          |                                         Ajoute l’option **/noguiboot** au <OSEntryLineNum>spécifié, en désactivant la barre de progression qui s’affiche avant l’invite d’ouverture de session Ctrl + Alt + Suppr.                                          |
| /ID <OSEntryLineNum> | Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini auquel les options de chargement du système d’exploitation sont ajoutées. La première ligne après l’en-tête de la section [Operating Systems] est 1. |
|          /?          |                                                                                               Affiche l'aide à l'invite de commandes.                                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande **bootcfg/Addsw** :
```
bootcfg /addsw /mm 64 /id 2 
bootcfg /addsw /so /id 3 
bootcfg /addsw /so /ng /s srvmain /u hiropln /id 2 
bootcfg /addsw /ng /id 2 
bootcfg /addsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
