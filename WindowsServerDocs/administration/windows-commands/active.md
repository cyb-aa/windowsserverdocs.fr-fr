---
title: active
description: Rubrique de commandes de Windows pour **active** - disques de base, marque la partition avec le focus comme active.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a039e0200fb84d446739ac7017556b6c302f4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868760"
---
# <a name="active"></a>active



Disques de base, marque la partition avec le focus comme active.

> [!CAUTION]
> DiskPart vérifie uniquement que la partition est capable de contenir les fichiers de démarrage du système d’exploitation. DiskPart ne vérifie pas le contenu de la partition. Si vous marquez par erreur une partition comme active et il ne contient-elle pas les fichiers de démarrage du système d’exploitation, votre ordinateur ne peut pas démarrer.

## <a name="syntax"></a>Syntaxe

```
active
```

## <a name="remarks"></a>Notes

-   Cela informe le système de base d’entrée/sortie (BIOS) ou l’Interface EFI (Extensible Firmware) que la partition ou le volume est une partition système valide ou le volume système.
-   Seules les partitions peuvent être marquées comme actives.
-   Une partition doit être sélectionnée pour cette opération réussisse. Utilisez le **sélectionnez partition** commande pour sélectionner une partition et déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour marquer la partition avec le focus comme partition active, tapez :
```
active
```

#### <a name="additional-references"></a>Références supplémentaires

