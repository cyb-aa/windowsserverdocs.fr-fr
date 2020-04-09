---
title: supprimer le disque
description: La rubrique commandes Windows pour supprimer un disque, qui supprime un disque dynamique manquant de la liste des disques.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a767e0689d5fbabb193df37528a0909ab63a1ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846652"
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

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour supprimer un disque dynamique manquant de la liste des disques, tapez :
```
delete disk
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

