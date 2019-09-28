---
title: active
description: La rubrique commandes Windows pour les disques de base **actifs** , marque la partition avec le focus comme active.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c926bf9b7a583cf7eaa23166e09e6f0a1599e625
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382850"
---
# <a name="active"></a>active



Sur les disques de base, marque la partition ayant le focus comme active.

> [!CAUTION]
> DiskPart vérifie uniquement que la partition est capable de contenir les fichiers de démarrage du système d’exploitation. DiskPart ne vérifie pas le contenu de la partition. Si, par erreur, vous marquez une partition comme active et qu’elle ne contient pas les fichiers de démarrage du système d’exploitation, il se peut que votre ordinateur ne démarre pas.

## <a name="syntax"></a>Syntaxe

```
active
```

## <a name="remarks"></a>Notes

-   Cela indique au système BIOS ou Extensible Firmware Interface (EFI) de base que la partition ou le volume est une partition système ou un volume système valide.
-   Seules les partitions peuvent être marquées comme actives.
-   Une partition doit être sélectionnée pour que cette opération aboutisse. Utilisez la commande **Sélectionner une partition** pour sélectionner une partition et y déplacer le focus.

## <a name="BKMK_examples"></a>Illustre

Pour marquer la partition ayant le focus sur la partition active, tapez :
```
active
```

#### <a name="additional-references"></a>Références supplémentaires

