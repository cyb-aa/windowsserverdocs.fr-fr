---
title: convert basic
description: Rubrique de référence pour la commande Convert Basic, qui convertit un disque dynamique vide en disque de base.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e44ecc9f5d18bbe426c63f8854e7c3347f418bb2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720794"
---
# <a name="convert-basic"></a>convert basic

Convertit un disque dynamique vide en disque de base. Pour que cette opération aboutisse, vous devez sélectionner un disque dynamique. Utilisez la [commande Sélectionner le disque](select-disk.md) pour sélectionner un disque dynamique et lui faire passer le focus.

> [!IMPORTANT]
> Le disque doit être vide pour pouvoir être converti en disque de base. Sauvegardez vos données, puis supprimez toutes les partitions ou tous les volumes avant de convertir le disque.

> [!NOTE]
> Pour obtenir des instructions sur l’utilisation de cette commande, consultez la [page modification d’un disque dynamique en disque de base](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755238(v=ws.11))).

## <a name="syntax"></a>Syntaxe

```
convert basic [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="examples"></a>Exemples

Pour convertir le disque dynamique sélectionné en base, tapez :

```
convert basic
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Convert](convert.md)
