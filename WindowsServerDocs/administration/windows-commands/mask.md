---
title: masque
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 816bcd932091b33ed897add5a13603e3a1eea925
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724017"
---
# <a name="mask"></a>masque



Supprime les clichés instantanés matériels importés à l’aide de la commande **Importer** .



## <a name="syntax"></a>Syntaxe

```
mask <ShadowSetID>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|ShadowSetID|Supprime les clichés instantanés qui appartiennent à l’ID du jeu de clichés instantanés spécifié.|

## <a name="remarks"></a>Notes 

-   Vous pouvez utiliser un alias existant ou une variable d’environnement à la place de *ShadowSetID*. Utilisez **Ajouter** sans paramètres pour afficher les alias existants.

## <a name="examples"></a>Exemples

Pour supprimer le cliché instantané importé% Import_1%, tapez :
```
mask %Import_1%
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)