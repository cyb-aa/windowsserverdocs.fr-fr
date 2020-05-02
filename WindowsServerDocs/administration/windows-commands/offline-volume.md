---
title: volume hors connexion
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d17295f3367fed054a7f6a245bae44ea3494a4a8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723450"
---
# <a name="offline-volume"></a>volume hors connexion



Met le volume en ligne avec le focus à l’état hors connexion.

> [!IMPORTANT]
> Cette commande DiskPart n’est pas disponible dans les éditions de Windows Vista.

## <a name="syntax"></a>Syntaxe

```
offline volume [noerr]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes 

-   Pour que cela aboutisse, vous devez sélectionner un volume. Utilisez la commande **Sélectionner un volume** pour sélectionner un disque et lui déplacer le focus.

## <a name="examples"></a>Exemples

Pour mettre le disque avec le focus hors connexion, tapez :
```
offline volume
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

