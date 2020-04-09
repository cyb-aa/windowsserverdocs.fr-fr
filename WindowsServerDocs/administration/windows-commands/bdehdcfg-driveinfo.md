---
title: BdeHdCfg DriveInfo
description: La rubrique commandes Windows pour **BdeHdCfg DriveInfo**, qui affiche la lettre de lecteur, la taille totale, l’espace libre maximal et les caractéristiques de la partition.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c598ea2d1d140090d623b3b48dbcc1be51ee66c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851062"
---
# <a name="bdehdcfg-driveinfo"></a>BdeHdCfg : DriveInfo

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche la lettre de lecteur, la taille totale, l’espace libre maximal et les caractéristiques de la partition. Seules les partitions valides apparaissent. L'espace non alloué n'apparaît pas si quatre partitions principales ou étendues existent déjà.

Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -driveinfo <DriveLetter>
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| <DriveLetter> | Spécifie une lettre de lecteur suivie d’un signe deux-points. |

## <a name="remarks"></a>Notes

La commande est à titre d’information uniquement et ne modifie pas le lecteur.

## <a name="example"></a><a name=BKMK_Examples></a>Tels

L’exemple suivant affiche les informations sur le lecteur C.

```
bdehdcfg  driveinfo C:
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
