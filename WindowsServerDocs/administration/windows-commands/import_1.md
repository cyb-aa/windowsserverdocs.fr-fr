---
title: importer
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 569d986c57ae8b3d7253c050146ac0583c7c92df
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724843"
---
# <a name="import"></a>importer



Importe un groupe de disques étrangers dans le groupe de disques de l’ordinateur local.

## <a name="syntax"></a>Syntaxe

```
import [noerr]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes 

-   La commande Import importe chaque disque qui se trouve dans le même groupe que le disque qui a le focus.
-   Vous devez sélectionner un disque dynamique dans un groupe de disques étrangers pour que cette opération aboutisse. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.

## <a name="examples"></a>Exemples

Pour importer chaque disque qui se trouve dans le même groupe de disques que le disque ayant le focus dans le groupe de disques de l’ordinateur local, tapez :
```
import
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

