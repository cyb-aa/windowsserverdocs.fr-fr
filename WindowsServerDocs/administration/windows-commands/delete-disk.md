---
title: Suppression du disque
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e401133854118e82a45fd5e01466288ae41f67da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863910"
---
# <a name="delete-disk"></a>Suppression du disque



Supprime un disque dynamique manquant dans la liste des disques.

Pour obtenir des instructions concernant l’utilisation de cette commande, consultez [supprimer un disque dynamique manquant](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Syntaxe

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|
|override|Permet à DiskPart de supprimer tous les volumes simples sur le disque. Si le disque contient la moitié d’un volume en miroir, la moitié de la mise en miroir sur le disque est supprimée. La commande de remplacement de disque de suppression échoue si le disque est un membre d’un volume RAID-5.|

## <a name="BKMK_examples"></a>Exemples

Pour supprimer un disque dynamique manquant dans la liste des disques, tapez :
```
delete disk
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

