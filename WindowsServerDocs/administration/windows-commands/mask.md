---
title: Filtrage
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2f0dc83d7d9f7204f56e95c62b7cfad991f539ef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373712"
---
# <a name="mask"></a>Filtrage



supprime les clichés instantanés matériels importés à l’aide de la commande **Importer** .

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
mask <ShadowSetID>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|ShadowSetID|Supprime les clichés instantanés qui appartiennent à l’ID du jeu de clichés instantanés spécifié.|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser un alias existant ou une variable d’environnement à la place de *ShadowSetID*. Utilisez **Ajouter** sans paramètres pour afficher les alias existants.

## <a name="BKMK_examples"></a>Illustre

Pour supprimer le cliché instantané importé% Import_1%, tapez :
```
mask %Import_1%
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)