---
title: supprimer le disque
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 886e84edf80227d42ad77e36aed388b9e853ca62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378668"
---
# <a name="delete-disk"></a>supprimer le disque



Supprime un disque dynamique manquant de la liste des disques.

Pour obtenir des instructions sur l’utilisation de cette commande, consultez [supprimer un disque dynamique manquant](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Syntaxe

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|
|remplacer|Permet à DiskPart de supprimer tous les volumes simples sur le disque. Si le disque contient la moitié d’un volume en miroir, la moitié du miroir sur le disque est supprimée. La commande delete disk override échoue si le disque est membre d’un volume RAID-5.|

## <a name="BKMK_examples"></a>Illustre

Pour supprimer un disque dynamique manquant de la liste des disques, tapez :
```
delete disk
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

