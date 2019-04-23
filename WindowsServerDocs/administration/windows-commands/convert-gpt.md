---
title: convert gpt
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e838f68162b6faabf2ecbc7dea2ce840235890c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859110"
---
# <a name="convert-gpt"></a>convert gpt



Convertit un disque de base vide avec un style de partition à secteur de démarrage principal (MBR) en un disque de base avec le style de partition GUID partition GPT (table).

Pour obtenir des instructions concernant l’utilisation de cette commande, consultez [modifier un disque Master Boot Record vers un disque de Table de Partition GUID](https://go.microsoft.com/fwlink/?LinkId=207049) (https://go.microsoft.com/fwlink/?LinkId=207049).

## <a name="syntax"></a>Syntaxe

```
convert gpt [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|

## <a name="remarks"></a>Notes

> [!IMPORTANT]
> Le disque doit être vide pour le convertir en disque GPT. Sauvegarder vos données, puis supprimez toutes les partitions ou volumes avant de convertir le disque.
-   La taille de disque minimal requis pour la conversion au format GPT est 128 mégaoctets.
-   Un disque MBR de base doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque de base et de déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour convertir un disque de base à partir du style de partition MBR style de partition GPT, tapez :
```
convert gpt
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

