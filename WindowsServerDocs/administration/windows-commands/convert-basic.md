---
title: convert basic
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: df1f999499154366304d59e0573ba921ab1af83d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434240"
---
# <a name="convert-basic"></a>convert basic



Convertit un disque dynamique vide à un disque de base.

Pour obtenir des instructions concernant l’utilisation de cette commande, consultez [modifier un disque dynamique en disque de base](https://go.microsoft.com/fwlink/?LinkId=207048) (https://go.microsoft.com/fwlink/?LinkId=207048).

## <a name="syntax"></a>Syntaxe

```
convert basic [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|

## <a name="remarks"></a>Notes

> [!IMPORTANT]
> Le disque doit être vide pour le convertir en un disque de base. Sauvegarder vos données, puis supprimez toutes les partitions ou volumes avant de convertir le disque.
> -   Un disque dynamique doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque dynamique et déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour convertir le disque dynamique sélectionné basic, tapez :
```
convert basic
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

