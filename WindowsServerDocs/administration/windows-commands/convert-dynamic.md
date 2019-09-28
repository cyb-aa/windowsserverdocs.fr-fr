---
title: convertir dynamique
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15c15d14aeb440c5d7862f0a304f223988f52bbe
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379099"
---
# <a name="convert-dynamic"></a>convertir dynamique



Convertit un disque de base en disque dynamique.

Pour obtenir des instructions sur l’utilisation de cette commande, consultez [modifier un disque de base en disque dynamique](https://go.microsoft.com/fwlink/?LinkId=207047) (https://go.microsoft.com/fwlink/?LinkId=207047).

## <a name="syntax"></a>Syntaxe

```
convert dynamic [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

-   Les partitions existantes sur le disque de base deviennent des volumes simples.
-   Pour que cette opération aboutisse, vous devez sélectionner un disque de base. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque de base et décaler le focus vers celui-ci.

## <a name="BKMK_examples"></a>Illustre

Pour convertir un disque de base en disque dynamique, tapez :
```
convert dynamic
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

