---
title: list
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: aacc93e1c7a16a7327ddbd17515f19cf41a5b458
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825540"
---
# <a name="list"></a>list



Répertorie les enregistreurs, clichés instantanés ou actuellement des fournisseurs qui se trouvent sur le système. Si utilisée sans paramètres, **liste** affiche l’aide à l’invite de commandes.

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
|enregistreurs|Répertorie les enregistreurs. Consultez [List writers](list-writers.md) pour la syntaxe et les paramètres.|
|Shadows|Répertorie les persistant et existantes non persistant clichés instantanés. Consultez [liste shadows](list-shadows.md) pour la syntaxe et les paramètres.|
|fournisseurs|Listes actuellement enregistré des fournisseurs de clichés instantanés. Consultez [répertorier fournisseurs](list-providers.md) pour la syntaxe et les paramètres.|

## <a name="BKMK_examples"></a>Exemples

Pour répertorier tous les clichés instantanés, tapez :
```
list shadows all
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)