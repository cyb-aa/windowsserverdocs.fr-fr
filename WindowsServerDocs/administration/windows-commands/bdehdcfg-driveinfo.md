---
title: BdeHdCfg DriveInfo
description: Rubrique de référence pour la commande BdeHdCfg DriveInfo, qui affiche la lettre de lecteur, la taille totale, l’espace libre maximal et les caractéristiques de la partition.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b18b4c3e128cd17353d369b418a049d0208cb654
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718690"
---
# <a name="bdehdcfg-driveinfo"></a>BdeHdCfg : DriveInfo

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche la lettre de lecteur, la taille totale, l’espace libre maximal et les caractéristiques de la partition. Seules les partitions valides apparaissent. L'espace non alloué n'apparaît pas si quatre partitions principales ou étendues existent déjà.

>[!NOTE]
> Cette commande est à titre d’information uniquement et n’apporte aucune modification au lecteur.

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -driveinfo <drive_letter>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| <drive_letter> | Spécifie une lettre de lecteur suivie d’un signe deux-points. |

## <a name="example"></a> Exemple

Pour afficher les informations sur le lecteur C ::

```
bdehdcfg  driveinfo C:
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
