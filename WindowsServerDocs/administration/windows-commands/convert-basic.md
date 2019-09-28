---
title: convert basic
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4b81126f4a623d841bb5868f786678d7b093581
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379130"
---
# <a name="convert-basic"></a>convert basic



Convertit un disque dynamique vide en disque de base.

Pour obtenir des instructions sur l’utilisation de cette commande, consultez la [page Modifier un disque dynamique en disque de base](https://go.microsoft.com/fwlink/?LinkId=207048) (https://go.microsoft.com/fwlink/?LinkId=207048).

## <a name="syntax"></a>Syntaxe

```
convert basic [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

> [!IMPORTANT]
> Le disque doit être vide pour pouvoir être converti en disque de base. Sauvegardez vos données, puis supprimez toutes les partitions ou tous les volumes avant de convertir le disque.
> -   Pour que cette opération aboutisse, vous devez sélectionner un disque dynamique. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque dynamique et lui faire passer le focus.

## <a name="BKMK_examples"></a>Illustre

Pour convertir le disque dynamique sélectionné en base, tapez :
```
convert basic
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

