---
title: supprimer le disque
description: Rubrique de référence sur la suppression d’un disque, qui supprime un disque dynamique manquant de la liste des disques.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad4888835c0bb1862344f104099b8b59027d1de9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716753"
---
# <a name="delete-disk"></a>supprimer le disque

Supprime un disque dynamique manquant de la liste des disques.

Pour obtenir des instructions sur l’utilisation de cette commande, consultez [supprimer un disque dynamique manquant](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Syntaxe

```
delete disk [noerr] [override]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|
|override|Permet à DiskPart de supprimer tous les volumes simples sur le disque. Si le disque contient la moitié d’un volume en miroir, la moitié du miroir sur le disque est supprimée. La commande delete disk override échoue si le disque est membre d’un volume RAID-5.|

## <a name="examples"></a>Exemples

Pour supprimer un disque dynamique manquant de la liste des disques, tapez :
```
delete disk
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

