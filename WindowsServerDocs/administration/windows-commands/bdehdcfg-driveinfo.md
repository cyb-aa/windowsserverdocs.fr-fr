---
title: BdeHdCfg DriveInfo
description: 'Rubrique relative aux commandes Windows pour * * BdeHdCfg : DriveInfo * *-affiche la lettre de lecteur, la taille totale, l’espace libre maximal et les caractéristiques de la partition.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0f4541bfd71fb7639d18e6e548559ed02918815
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382270"
---
# <a name="bdehdcfg-driveinfo"></a>BdeHdCfg : DriveInfo

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche la lettre de lecteur, la taille totale, l’espace libre maximal et les caractéristiques de la partition. Seules les partitions valides apparaissent. L'espace non alloué n'apparaît pas si quatre partitions principales ou étendues existent déjà. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).
## <a name="syntax"></a>Syntaxe
```
bdehdcfg -driveinfo <DriveLetter>
```
### <a name="parameters"></a>Paramètres

|   Paramètre   |                  Description                  |
|---------------|-----------------------------------------------|
| <DriveLetter> | Spécifie une lettre de lecteur suivie d’un signe deux-points. |

## <a name="remarks"></a>Notes
La commande est à titre d’information uniquement et ne modifie pas le lecteur.
## <a name="BKMK_Examples"></a>Tels
L’exemple suivant affiche les informations sur le lecteur C.
```
bdehdcfg  driveinfo C:
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [bdehdcfg](bdehdcfg.md)
