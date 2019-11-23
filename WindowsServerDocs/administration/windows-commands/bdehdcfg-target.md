---
title: cible BdeHdCfg
description: 'Rubrique relative aux commandes Windows pour la cible BdeHdCfg : prépare une partition à utiliser comme lecteur système par BitLocker et la récupération Windows.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2fb0a1daa257ef2c9f1cd77b88e5ef14f84a0dfa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382182"
---
# <a name="bdehdcfg-target"></a>BdeHdCfg : cible



Prépare une partition à utiliser comme lecteur système par BitLocker et la récupération Windows. Par défaut, cette partition est créée sans lettre de lecteur. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|par défaut|Indique que l'outil en ligne de commande suivra le même processus que l'Assistant d'installation BitLocker.|
|unallocated|Crée la partition système hors de l'espace non alloué disponible sur le disque.|
|\<lettre_lecteur > Shrink|Réduit le lecteur spécifié selon la quantité nécessaire pour créer une partition système active. Pour utiliser cette commande, le lecteur spécifié doit avoir au moins 5 pour cent d'espace libre.|
|\<lettre_lecteur > fusion|Utilise le lecteur spécifié comme partition système active. Le lecteur de système d'exploitation ne peut pas être une cible pour la fusion.|

## <a name="BKMK_Examples"></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **target** pour désigner un lecteur existant (P) comme lecteur système.
```
bdehdcfg -target P: merge
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)