---
title: diskperf
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 829f0284d761e6a5134011fa1dff99646d55fc13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377811"
---
# <a name="diskperf"></a>diskperf



Dans Windows 2000, les compteurs de performances de disque logique et physique ne sont pas activés par défaut.

**Diskperf** est inclus dans Windows XP, windows Server 2003, windows Server 2008, Windows Vista, windows Server 2008 R2 et Windows 7 afin qu’il puisse être utilisé pour activer ou désactiver à distance des compteurs de performances de disque logique ou physique sur des ordinateurs exécutant Windows 2000.

## <a name="syntax"></a>Syntaxe

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>Options

|Option|Description|
|------|-----------|
|-?|Affiche l’aide contextuelle.|
|-Y|Démarrez tous les compteurs de performances de disque au redémarrage de l’ordinateur.|
|-YD|Activez les compteurs de performances de disque pour les lecteurs physiques au redémarrage de l’ordinateur.|
|-YV|Activez les compteurs de performances de disque pour les lecteurs logiques ou les volumes de stockage au redémarrage de l’ordinateur.|
|-N|Désactivez tous les compteurs de performances de disque au redémarrage de l’ordinateur.|
|-ND|Désactivez les compteurs de performances de disque pour les lecteurs physiques au redémarrage de l’ordinateur.|
|-NV|Désactivez les compteurs de performances de disque pour les lecteurs logiques ou les volumes de stockage au redémarrage de l’ordinateur.|
|\\ @ no__t-1 *\<computername >*|Spécifiez le nom de l’ordinateur sur lequel vous souhaitez activer ou désactiver les compteurs de performances de disque.|