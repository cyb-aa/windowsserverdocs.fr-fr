---
title: actif
description: La rubrique commandes Windows pour **active**, qui sur les disques de base, marque la partition avec le focus comme active.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42f2e0d367344355e8f9a570f37cfbdc5dfc4590
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851372"
---
# <a name="active"></a>actif

Sur les disques de base, marque la partition ayant le focus comme active.

> [!CAUTION]
> DiskPart vérifie uniquement que la partition est capable de contenir les fichiers de démarrage du système d’exploitation. DiskPart ne vérifie pas le contenu de la partition. Si, par erreur, vous marquez une partition comme active et qu’elle ne contient pas les fichiers de démarrage du système d’exploitation, il se peut que votre ordinateur ne démarre pas.

## <a name="syntax"></a>Syntaxe

```
active
```- 

## Remarks

-   This informs the basic input/output system (BIOS) or Extensible Firmware Interface (EFI) that the partition or volume is a valid system partition or system volume.

-   Only partitions can be marked as active.

-   A partition must be selected for this operation to succeed. Use the **select partition** command to select a partition and shift the focus to it.

## <a name=BKMK_examples></a>Examples

To mark the partition with focus as the active partition, type:

```
actif
```
## Additional References

- [Command-Line Syntax Key](command-line-syntax-key.md)