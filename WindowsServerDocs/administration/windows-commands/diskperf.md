---
title: diskperf
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a99f18b56c9295e902a3c89e2e89b36c9c1b6c89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852580"
---
# <a name="diskperf"></a>diskperf



Dans Windows 2000, les compteurs de performances de disques physiques et logiques ne sont pas activés par défaut.

**Diskperf** est inclus dans Windows XP, Windows Server 2003, Windows Server 2008, Windows Vista, Windows Server 2008 R2 et Windows 7 afin qu’il peut être utilisé pour activer ou désactiver les compteurs de performances disque physique ou logique sur les ordinateurs exécutant à distance Windows 2000.

## <a name="syntax"></a>Syntaxe

```
diskperf [-Y[D|V] | -N[D|V]] [\\computername]
```

## <a name="options"></a>Options

|Option|Description|
|------|-----------|
|-?|Affiche sensible au contexte d’aide.|
|-Y|Démarrer tous les compteurs de performances de disque lorsque l’ordinateur redémarre.|
|-YD|Activer les compteurs de performances de disque pour les lecteurs physiques lorsque l’ordinateur redémarre.|
|-YV|Activer les compteurs de performances de disque pour les lecteurs logiques ou les volumes de stockage lorsque l’ordinateur redémarre.|
|-N|Désactiver tous les compteurs de performances de disque lorsque l’ordinateur redémarre.|
|-ND|Désactiver les compteurs de performances de disque pour les lecteurs physiques lorsque l’ordinateur redémarre.|
|-NV|Désactiver les compteurs de performances de disque pour les lecteurs logiques ou les volumes de stockage lorsque l’ordinateur redémarre.|
|\\\\*\<computername>*|Spécifiez le nom de l’ordinateur où vous souhaitez activer ou désactiver les compteurs de performances de disque.|