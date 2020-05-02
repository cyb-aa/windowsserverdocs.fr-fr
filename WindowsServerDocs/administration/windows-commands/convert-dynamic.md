---
title: convertir dynamique
description: Rubrique de référence pour la commande dynamique Convert, qui convertit un disque de base en disque dynamique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 05d507fb5a1f8ac3ca8d8899249a26dee496ed2a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720777"
---
# <a name="convert-dynamic"></a>convertir dynamique

Convertit un disque de base en disque dynamique. Pour que cette opération aboutisse, vous devez sélectionner un disque de base. Utilisez la [commande Sélectionner le disque](select-disk.md) pour sélectionner un disque de base et décaler le focus vers celui-ci.

> [!NOTE]
> Pour obtenir des instructions sur l’utilisation de cette commande, consultez la [page modification d’un disque dynamique en disque de base](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755238(v=ws.11))).

## <a name="syntax"></a>Syntaxe

```
convert dynamic [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

#### <a name="remarks"></a>Notes 

- Les partitions existantes sur le disque de base deviennent des volumes simples.

## <a name="examples"></a>Exemples

Pour convertir un disque de base en disque dynamique, tapez :

```
convert dynamic
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Convert](convert.md)
