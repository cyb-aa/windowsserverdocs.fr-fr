---
title: convert basic
description: La rubrique commandes Windows pour Convert Basic, qui convertit un disque dynamique vide en disque de base.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e0f6f5f04373042956d83bc9136c884c268e591
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847312"
---
# <a name="convert-basic"></a>convert basic

Convertit un disque dynamique vide en disque de base.

Pour obtenir des instructions sur l’utilisation de cette commande, consultez la [page modification d’un disque dynamique en disque de base](https://go.microsoft.com/fwlink/?LinkId=207048) (https://go.microsoft.com/fwlink/?LinkId=207048).

## <a name="syntax"></a>Syntaxe

```
convert basic [noerr]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

> [!IMPORTANT]
> Le disque doit être vide pour pouvoir être converti en disque de base. Sauvegardez vos données, puis supprimez toutes les partitions ou tous les volumes avant de convertir le disque.

-   Pour que cette opération aboutisse, vous devez sélectionner un disque dynamique. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque dynamique et lui faire passer le focus.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour convertir le disque dynamique sélectionné en base, tapez :
```
convert basic
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

