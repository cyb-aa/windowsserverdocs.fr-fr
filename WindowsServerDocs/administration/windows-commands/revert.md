---
title: rétablir
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3243f13a4997824d9fff7c874ce26d56325fefa4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371453"
---
# <a name="revert"></a>rétablir



Ramène un volume à un cliché instantané spécifié. Cela est pris en charge uniquement pour les clichés instantanés dans le contexte CLIENTACCESSIBLE. Ces clichés instantanés sont persistants et peuvent être effectués uniquement par le fournisseur système. S’il est utilisé sans paramètres, l’option **rétablir** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
revert <ShadowCopyID>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ShadowCopyID >|Spécifie l’ID de cliché instantané pour rétablir le volume.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)