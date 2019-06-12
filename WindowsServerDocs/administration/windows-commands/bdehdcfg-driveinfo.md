---
title: bdehdcfg driveinfo
description: 'Rubrique de commandes de Windows pour ** bdehdcfg : driveinfo ** - affiche la lettre de lecteur, la taille totale, l’espace libre maximal et les caractéristiques de la partition.'
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b2dd62e34f8205e0b5d395ba759fff4b4937b0ad
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435045"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg: driveinfo

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche la lettre de lecteur, la taille totale, l’espace libre maximal et les caractéristiques de la partition. Seules les partitions valides apparaissent. L'espace non alloué n'apparaît pas si quatre partitions principales ou étendues existent déjà. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).
## <a name="syntax"></a>Syntaxe
```
bdehdcfg -driveinfo <DriveLetter>
```
### <a name="parameters"></a>Paramètres

|   Paramètre   |                  Description                  |
|---------------|-----------------------------------------------|
| <DriveLetter> | Spécifie une lettre de lecteur suivie du signe deux-points. |

## <a name="remarks"></a>Notes
La commande est d’information uniquement et ne rend aucune modification sur le lecteur.
## <a name="BKMK_Examples"></a>Exemple
L’exemple suivant affiche les informations de lecteur pour le lecteur C.
```
bdehdcfg  driveinfo C:
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [bdehdcfg](bdehdcfg.md)
