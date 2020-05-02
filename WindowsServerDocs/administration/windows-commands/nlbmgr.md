---
title: nlbmgr
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59ec2ad6b4614f89f9c1c3cbda97d5283a2374bd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723739"
---
# <a name="nlbmgr"></a>nlbmgr

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

À l’aide du gestionnaire d’équilibrage de la charge réseau, vous pouvez configurer et gérer vos clusters d’équilibrage de charge réseau et tous les ordinateurs hôtes du cluster à partir d’un seul ordinateur. vous pouvez également répliquer la configuration du cluster sur d’autres hôtes. Vous pouvez démarrer le gestionnaire d’équilibrage de la charge réseau à partir de la ligne de commande à l’aide de la commande **Nlbmgr. exe**, qui est installée dans le dossier **systemroot\system32** .
## <a name="syntax"></a>Syntaxe
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
#### <a name="parameters"></a>Paramètres

|        Paramètre        |                                                                                                                                                                                                Description                                                                                                                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /help          |                                                                                                                                                                                   Affiche l'aide à l'invite de commandes.                                                                                                                                                                                    |
|         /noping         | Empêche le gestionnaire d’équilibrage de la charge réseau d’exécuter une commande ping sur les ordinateurs hôtes avant d’essayer de les contacter via Windows Management Instrumentation (WMI). Utilisez cette option si vous avez désactivé le protocole ICMP (Internet Control Message Protocol) sur toutes les cartes réseau disponibles. Si le gestionnaire d’équilibrage de la charge réseau tente de contacter un ordinateur hôte qui n’est pas disponible, vous rencontrerez un délai lors de l’utilisation de cette option. |
|  /hostlist<filename>   |                                                                                                                                                                Charge les ordinateurs hôtes spécifiés en tant que nom de fichier dans le gestionnaire d’équilibrage de charge réseau.                                                                                                                                                                 |
| /autorefresh<interval> |                                                                                                          Force le gestionnaire d’équilibrage de la charge réseau à actualiser ses <interval> informations sur l’hôte et le cluster toutes les secondes. Si aucun intervalle n’est spécifié, les informations sont actualisées toutes les 60 secondes.                                                                                                          |
|           /?            |                                                                                                                                                                                   Affiche l'aide à l'invite de commandes.                                                                                                                                                                                    |

## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

