---
title: list
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b6c8d0-872e-4dba-9715-1db8ab892e98
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91b42925fc822b10157bb488167d06fe82cfe1e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374701"
---
# <a name="list"></a>list



répertorie les writers, les clichés instantanés ou les fournisseurs de clichés instantanés actuellement enregistrés qui se trouvent sur le système. S’il est utilisé sans paramètres, la commande **liste** affiche l’aide à l’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
list writers [metadata | detailed | status]
list shadows {all | set <SetID> | id <ShadowID>}
list providers
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|script|Répertorie les enregistreurs. Consultez [répertorier les enregistreurs](list-writers.md) pour la syntaxe et les paramètres.|
|Shadows|Répertorie les clichés instantanés non persistants permanents et existants. Consultez [liste des Shadows](list-shadows.md) pour la syntaxe et les paramètres.|
|éditeurs|Répertorie les fournisseurs de clichés instantanés actuellement inscrits. Consultez [répertorier les fournisseurs](list-providers.md) pour la syntaxe et les paramètres.|

## <a name="BKMK_examples"></a>Illustre

Pour répertorier tous les clichés instantanés, tapez :
```
list shadows all
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)