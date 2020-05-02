---
title: diskperf
description: Rubrique de référence pour diskperf, qui peut être utilisée pour activer ou désactiver à distance des compteurs de performances de disque logique ou physique sur des ordinateurs exécutant Windows 2000.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8f505d924ee1de311f2f2736ff65be844c3f2ea
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719444"
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
|-y|Démarrez tous les compteurs de performances de disque au redémarrage de l’ordinateur.|
|-YD|Activez les compteurs de performances de disque pour les lecteurs physiques au redémarrage de l’ordinateur.|
|-YV|Activez les compteurs de performances de disque pour les lecteurs logiques ou les volumes de stockage au redémarrage de l’ordinateur.|
|-n|Désactivez tous les compteurs de performances de disque au redémarrage de l’ordinateur.|
|-ND|Désactivez les compteurs de performances de disque pour les lecteurs physiques au redémarrage de l’ordinateur.|
|-NV|Désactivez les compteurs de performances de disque pour les lecteurs logiques ou les volumes de stockage au redémarrage de l’ordinateur.|
|\\\\*\<nom de l'>*|Spécifiez le nom de l’ordinateur sur lequel vous souhaitez activer ou désactiver les compteurs de performances de disque.|