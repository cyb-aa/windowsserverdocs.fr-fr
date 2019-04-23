---
title: bdehdcfg cible
description: Rubrique de commandes de Windows pour la cible, bdehdcfg prépare une partition pour une utilisation comme un lecteur du système par récupération BitLocker et Windows.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f8d180974f480b4c40532dab529ad49dcc33540d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881530"
---
# <a name="bdehdcfg-target"></a>Bdehdcfg : cible



Prépare une partition pour une utilisation comme un lecteur du système par BitLocker et la récupération de Windows. Par défaut, cette partition est créée sans lettre de lecteur. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|par défaut|Indique que l'outil en ligne de commande suivra le même processus que l'Assistant d'installation BitLocker.|
|unallocated|Crée la partition système hors de l'espace non alloué disponible sur le disque.|
|\<Lettre de lecteur > réduire|Réduit le lecteur spécifié selon la quantité nécessaire pour créer une partition système active. Pour utiliser cette commande, le lecteur spécifié doit avoir au moins 5 pour cent d'espace libre.|
|\<Lettre de lecteur > fusion|Utilise le lecteur spécifié comme partition système active. Le lecteur de système d'exploitation ne peut pas être une cible pour la fusion.|

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant décrit à l’aide de la **cible** commande pour désigner un lecteur existant (P) en tant que le lecteur du système.
```
bdehdcfg -target P: merge
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)