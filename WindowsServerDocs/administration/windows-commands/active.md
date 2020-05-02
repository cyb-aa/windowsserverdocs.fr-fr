---
title: active
description: Rubrique de référence pour la commande active, qui, sur les disques de base, marque la partition avec le focus comme active.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 997c57b93434738c87396812c9b5e5b12d7a8e89
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719016"
---
# <a name="active"></a>active

Sur les disques de base, marque la partition ayant le focus comme active. Seules les partitions peuvent être marquées comme actives. Une partition doit être sélectionnée pour que cette opération aboutisse. Utilisez la commande **Sélectionner une partition** pour sélectionner une partition et y déplacer le focus.

> [!CAUTION]
> DiskPart informe uniquement le système BIOS ou Extensible Firmware Interface (EFI) de base que la partition ou le volume est une partition système ou un volume système valide, et qu’il est capable de contenir les fichiers de démarrage du système d’exploitation. DiskPart ne vérifie pas le contenu de la partition. Si, par erreur, vous marquez une partition comme active et qu’elle ne contient pas les fichiers de démarrage du système d’exploitation, il se peut que votre ordinateur ne démarre pas.

## <a name="syntax"></a>Syntaxe

```
active
```

## <a name="examples"></a>Exemples

Pour marquer la partition ayant le focus sur la partition active, tapez :

```
active
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [sélectionner une partition, commande](select-partition.md)
