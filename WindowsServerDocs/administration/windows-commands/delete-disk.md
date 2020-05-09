---
title: supprimer le disque
description: Rubrique de référence pour la commande delete disk, qui supprime un disque dynamique manquant de la liste des disques.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c8076f486251e428bce8805e15c2aa74caaf834
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993139"
---
# <a name="delete-disk"></a>supprimer le disque

Supprime un disque dynamique manquant de la liste des disques.

> [!NOTE]
> Pour obtenir des instructions détaillées sur l’utilisation de cette commande, consultez [supprimer un disque dynamique manquant](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753029(v=ws.11)).

## <a name="syntax"></a>Syntaxe

```
delete disk [noerr] [override]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |
| override | Permet à DiskPart de supprimer tous les volumes simples sur le disque. Si le disque contient la moitié d’un volume en miroir, la moitié du miroir sur le disque est supprimée. La commande delete disk override échoue si le disque est membre d’un volume RAID-5. |

## <a name="examples"></a>Exemples

Pour supprimer un disque dynamique manquant de la liste des disques, tapez :

```
delete disk
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Delete](delete.md)
