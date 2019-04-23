---
title: sans perte de données
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4c3723dd414adca68c22011ef3f6be02eb6531d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889900"
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
|[Sélectionnez le disque](select-disk.md)|Déplace le focus sur un disque.|
|[Sélectionnez la partition](select-partition.md)|Déplace le focus sur une partition.|
|[Sélectionnez le volume](select-volume.md)|Déplace le focus sur un volume.|
|[Sélectionnez vdisk](select-vdisk.md)|Déplace le focus sur un disque dur virtuel.|

## <a name="remarks"></a>Notes

-   Si un volume est sélectionné avec une partition correspondante, la partition est automatiquement sélectionnée.
-   Si une partition est sélectionnée par un volume correspondant, le volume est automatiquement sélectionné.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

