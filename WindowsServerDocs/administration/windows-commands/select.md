---
title: sans perte de données
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7dc3bc8775f971968f096ba4344348e77c112cfa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384112"
---
# <a name="select"></a>sans perte de données



Déplace le focus sur un disque, une partition, un volume ou un disque dur virtuel (VHD).

## <a name="syntax"></a>Syntaxe

```
select disk
select partition
select volume
select vdisk
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[Sélectionner un disque](select-disk.md)|Déplace le focus sur un disque.|
|[Sélectionner une partition](select-partition.md)|Déplace le focus vers une partition.|
|[Sélectionner le volume](select-volume.md)|Déplace le focus vers un volume.|
|[Sélectionner vdisk](select-vdisk.md)|Déplace le focus vers un disque dur virtuel.|

## <a name="remarks"></a>Notes

-   Si un volume est sélectionné avec une partition correspondante, la partition est automatiquement sélectionnée.
-   Si une partition est sélectionnée avec un volume correspondant, le volume est sélectionné automatiquement.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

