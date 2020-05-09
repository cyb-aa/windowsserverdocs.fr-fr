---
title: diskperf
description: Rubrique de référence pour la commande Diskperf, qui peut être utilisée pour activer ou désactiver à distance des compteurs de performances de disque logique ou physique sur des ordinateurs exécutant Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 092518f414d6e27436c46ffd6f9f15b6e6c0407e
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992428"
---
# <a name="diskperf"></a>diskperf

La commande **diskperf** active ou désactive à distance les compteurs de performance des disques physiques ou logiques sur les ordinateurs exécutant Windows.

## <a name="syntax"></a>Syntaxe

```
diskperf [-y[d|v] | -n[d|v]] [\\computername]
```

## <a name="options"></a>Options

| Option | Description |
| ------ | ----------- |
| -y | Démarre tous les compteurs de performances de disque au redémarrage de l’ordinateur. |
| -YD | Active les compteurs de performances de disque pour les lecteurs physiques au redémarrage de l’ordinateur. |
| -yv | Active les compteurs de performances de disque pour les lecteurs logiques ou les volumes de stockage au redémarrage de l’ordinateur. |
| -n | Désactive tous les compteurs de performances de disque au redémarrage de l’ordinateur. |
| -ND | Désactivez les compteurs de performances de disque pour les lecteurs physiques au redémarrage de l’ordinateur. |
| -NV | Désactivez les compteurs de performances de disque pour les lecteurs logiques ou les volumes de stockage au redémarrage de l’ordinateur. |
| `\\<computername>` | Spécifie le nom de l’ordinateur sur lequel vous souhaitez activer ou désactiver les compteurs de performances de disque. |
| -? | Affiche l’aide contextuelle. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
