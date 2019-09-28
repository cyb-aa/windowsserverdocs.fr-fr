---
title: convert mbr
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47001415158b3bdb0b06af9114b995a6f0634da1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379075"
---
# <a name="convert-mbr"></a>convert mbr



Convertit un disque de base vide avec le style de partition GPT (GUID partition table) en disque de base avec le style de partition enregistrement de démarrage principal (MBR).

> [!IMPORTANT]
> Le disque doit être vide pour pouvoir être converti en disque MBR. Sauvegardez vos données, puis supprimez toutes les partitions ou tous les volumes avant de convertir le disque.

Pour obtenir des instructions sur l’utilisation de cette commande, consultez [modifier un disque de table de partition GUID en disque d’enregistrement de démarrage principal](https://go.microsoft.com/fwlink/?LinkId=207050) (https://go.microsoft.com/fwlink/?LinkId=207050).

## <a name="syntax"></a>Syntaxe

```
convert mbr [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

-   Pour que cette opération aboutisse, vous devez sélectionner un disque de base. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque de base et décaler le focus vers celui-ci.

## <a name="BKMK_examples"></a>Illustre

Pour convertir un disque de base du style de partition GPT au style de partition MBR, tapez >:
```
convert mbr
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

