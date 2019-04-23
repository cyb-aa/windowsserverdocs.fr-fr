---
title: nlbmgr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ed11e702aeae66458f888e454c1bc1d1bc22630
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887080"
---
# <a name="nlbmgr"></a>nlbmgr

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

À l’aide du Gestionnaire d’équilibrage de charge réseau, vous pouvez configurer et gérer vos clusters d’équilibrage de charge réseau et de tous les hôtes du cluster à partir d’un seul ordinateur, et vous pouvez également répliquer la configuration du cluster vers d’autres hôtes. Vous pouvez démarrer le Gestionnaire d’équilibrage de charge réseau à partir de la ligne de commande à l’aide de la commande **nlbmgr.exe**, qui est installé dans le **systemroot\System32** dossier.
## <a name="syntax"></a>Syntaxe
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/help|Affiche l'aide à l'invite de commandes.|
|/noping|Empêche le Gestionnaire d’équilibrage de charge réseau à partir d’une commande ping les ordinateurs hôtes avant de les contacter via Windows Management Instrumentation (WMI). Utilisez cette option si vous avez désactivé le contrôle Message ICMP (Internet Protocol) sur toutes les cartes réseau disponibles. Si le Gestionnaire d’équilibrage de charge réseau tente de contacter un hôte qui n’est pas disponible, vous rencontrerez un retard lors de l’utilisation de cette option.|
|/hostlist <filename>|Charge les hôtes spécifiés dans le nom de fichier dans le Gestionnaire d’équilibrage de charge réseau.|
|/autorefresh <interval>|Provoque le Gestionnaire d’équilibrage de charge réseau actualiser ses informations d’hôte et cluster chaque <interval> secondes. Si aucun intervalle n’est spécifié, les informations sont actualisées toutes les 60 secondes.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

