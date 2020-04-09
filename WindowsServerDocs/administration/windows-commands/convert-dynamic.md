---
title: convertir dynamique
description: Rubrique relative aux commandes Windows pour convertir dynamique, qui convertit un disque de base en disque dynamique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ad84cf2ecb6808b68110187b52f3fc13590b491
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847322"
---
# <a name="convert-dynamic"></a>convertir dynamique

Convertit un disque de base en disque dynamique.

Pour obtenir des instructions sur l’utilisation de cette commande, consultez [modifier un disque de base en disque dynamique](https://go.microsoft.com/fwlink/?LinkId=207047) (https://go.microsoft.com/fwlink/?LinkId=207047).

## <a name="syntax"></a>Syntaxe

```
convert dynamic [noerr]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

-   Les partitions existantes sur le disque de base deviennent des volumes simples.
-   Pour que cette opération aboutisse, vous devez sélectionner un disque de base. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque de base et décaler le focus vers celui-ci.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour convertir un disque de base en disque dynamique, tapez :
```
convert dynamic
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

