---
title: filtrage
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1cb92caacb955449c1baaad411fdbe4cdf05b73
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839642"
---
# <a name="mask"></a>filtrage



supprime les clichés instantanés matériels importés à l’aide de la commande **Importer** .

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

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

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour supprimer le cliché instantané importé% Import_1%, tapez :
```
mask %Import_1%
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)