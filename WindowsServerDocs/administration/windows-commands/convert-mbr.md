---
title: convert mbr
description: La rubrique commandes Windows pour Convert MBR, qui convertit un disque de base vide avec le style de partition table de partition GUID (GPT) en un disque de base avec le style de partition d’enregistrement de démarrage principal (MBR).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eeaf79a380fb5f1074d2bbef004537804caa0d8d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847202"
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

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

-   Pour que cette opération aboutisse, vous devez sélectionner un disque de base. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque de base et décaler le focus vers celui-ci.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour convertir un disque de base du style de partition GPT au style de partition MBR, tapez >:
```
convert mbr
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

