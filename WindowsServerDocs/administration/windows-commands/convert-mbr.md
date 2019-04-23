---
title: convert mbr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: da8d62567863bc38a5aa0b35a8f3fe4ee24cc888
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834610"
---
# <a name="convert-mbr"></a>convert mbr



Convertit un disque de base vide avec le style de partition de Table de Partition GUID (GPT) en un disque de base avec le style de partition à secteur de démarrage principal (MBR).

> [!IMPORTANT]
> Le disque doit être vide pour le convertir en un disque MBR. Sauvegarder vos données, puis supprimez toutes les partitions ou volumes avant de convertir le disque.

Pour obtenir des instructions concernant l’utilisation de cette commande, consultez [modifier un disque GPT dans un disque Master Boot Record](https://go.microsoft.com/fwlink/?LinkId=207050) (https://go.microsoft.com/fwlink/?LinkId=207050).

## <a name="syntax"></a>Syntaxe

```
convert mbr [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|

## <a name="remarks"></a>Notes

-   Un disque de base doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque de base et de déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour convertir un disque de base à partir du style de partition GPT pour le style de partition MBR, tapez > :
```
convert mbr
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

