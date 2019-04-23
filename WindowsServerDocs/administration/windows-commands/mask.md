---
title: Masque
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 353e6080d1f6c548bc907b58655f31d0bce6de8b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858020"
---
# <a name="mask"></a>Masque



Supprime les clichés instantanés de matériels qui ont été importés à l’aide de la **importer** commande.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
mask <ShadowSetID>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|ShadowSetID|Supprime les clichés instantanés qui appartiennent à l’ID spécifié le jeu de copies de clichés instantanés.|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser un alias existant ou une variable d’environnement à la place de *ShadowSetID*. Utilisez **ajouter** sans paramètres pour voir les alias existants.

## <a name="BKMK_examples"></a>Exemples

Pour supprimer le clichés instantanés importés copie % Import_1, tapez :
```
mask %Import_1%
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)