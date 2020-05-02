---
title: bootcfg rmsw
description: Rubrique de référence pour la commande bootcfg Rmsw, qui supprime les options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fd7e4248-880e-4e2b-929e-87f8d44b9a63
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41c9819fb3d669b24a5918077bef960869625a15
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708915"
---
# <a name="bootcfg-rmsw"></a>bootcfg rmsw

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime les options de chargement du système d’exploitation pour une entrée de système d’exploitation spécifiée.

## <a name="syntax"></a>Syntaxe

```
bootcfg /rmsw [/s <computer> [/u <domain>\<user> /p <password>]] [/mm] [/bv] [/so] [/ng] /id <osentrylinenum>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `/s <computer>` | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. |
| `/u <domain>\<user>`  | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié `<user>` par `<domain>\<user>`ou. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande. |
| `/p <password>` | Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . |
| /mm | Supprime l’option/MAXMEM et la valeur de mémoire maximale associée du spécifié `<osentrylinenum>`. L’option/MAXMEM spécifie la quantité maximale de RAM que le système d’exploitation peut utiliser. |
| /bv | Supprime l’option/basevideo du spécifié `<osentrylinenum>`. L’option/basevideo indique au système d’exploitation d’utiliser le mode VGA standard pour le pilote vidéo installé. |
| /So | Supprime l’option/SOS du spécifié `<osentrylinenum>`. L’option/SOS indique au système d’exploitation d’afficher les noms des pilotes de périphériques pendant leur chargement. |
| /ng | Supprime l’option/noguiboot du spécifié `<osentrylinenum>`. L’option/noguiboot désactive la barre de progression qui s’affiche avant l’invite d’ouverture de session CTRL + ALT + SUPPR. |
| `/id <osentrylinenum>` | Spécifie le numéro de ligne d’entrée du système d’exploitation dans la section [Operating Systems] du fichier Boot. ini auquel les options de chargement du système d’exploitation sont ajoutées. La première ligne après l’en-tête de la section [Operating Systems] est 1. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour utiliser la commande **bootcfg/Rmsw** :

```
bootcfg /rmsw /mm 64 /id 2
bootcfg /rmsw /so /id 3
bootcfg /rmsw /so /ng /s srvmain /u hiropln /id 2
bootcfg /rmsw /ng /id 2
bootcfg /rmsw /mm 96 /ng /s srvmain /u maindom\hiropln /p p@ssW23 /id 2
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande bootcfg](bootcfg.md)
