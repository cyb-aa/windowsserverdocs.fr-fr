---
title: select
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9eeb40c0-4258-46e2-8dbc-94f63497e771
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9615918bb7fab45018f40b409427ab12fc3eddb7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721976"
---
# <a name="select"></a>select



Déplace le focus sur un disque, une partition, un volume ou un disque dur virtuel (VHD).

## <a name="syntax"></a>Syntaxe

```
select disk
select partition
select volume
select vdisk
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[Sélectionner un disque](select-disk.md)|Déplace le focus sur un disque.|
|[Sélectionner une partition](select-partition.md)|Déplace le focus vers une partition.|
|[Sélectionner le volume](select-volume.md)|Déplace le focus vers un volume.|
|[Sélectionner vdisk](select-vdisk.md)|Déplace le focus vers un disque dur virtuel.|

## <a name="remarks"></a>Notes 

-   Si un volume est sélectionné avec une partition correspondante, la partition est automatiquement sélectionnée.
-   Si une partition est sélectionnée avec un volume correspondant, le volume est sélectionné automatiquement.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

