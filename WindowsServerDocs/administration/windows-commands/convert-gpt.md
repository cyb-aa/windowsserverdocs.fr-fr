---
title: convert gpt
description: La rubrique commandes Windows pour Convert GPT, qui convertit un disque de base vide avec le style de partition d’enregistrement de démarrage principal (MBR) en disque de base avec le style de partition GPT (GUID partition table).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c1ffe61245f7752ccc81d21d513fa00acd7b68b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847282"
---
# <a name="convert-gpt"></a>convert gpt

Convertit un disque de base vide avec le style de partition d’enregistrement de démarrage principal (MBR) en disque de base avec le style de partition GPT (GUID partition table).

Pour obtenir des instructions sur l’utilisation de cette commande, consultez [modifier un disque d’enregistrement de démarrage principal en disque de table de partition GUID](https://go.microsoft.com/fwlink/?LinkId=207049) (https://go.microsoft.com/fwlink/?LinkId=207049).

## <a name="syntax"></a>Syntaxe

```
convert gpt [noerr]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

> [!IMPORTANT]
> Le disque doit être vide pour pouvoir être converti en disque GPT. Sauvegardez vos données, puis supprimez toutes les partitions ou tous les volumes avant de convertir le disque.
> -   La taille de disque minimale requise pour la conversion en GPT est de 128 mégaoctets.
> -   Vous devez sélectionner un disque MBR de base pour que cette opération aboutisse. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque de base et décaler le focus vers celui-ci.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour convertir un disque de base du style de partition MBR au style de partition GPT, tapez :
```
convert gpt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

