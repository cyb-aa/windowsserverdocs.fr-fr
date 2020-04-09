---
title: list
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57b6c8d0-872e-4dba-9715-1db8ab892e98
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d60c42b868a1e9a26e3168e4b489573f9f87e179
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841102"
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

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|script|Répertorie les enregistreurs. Consultez [répertorier les enregistreurs](list-writers.md) pour la syntaxe et les paramètres.|
|Shadows|Répertorie les clichés instantanés non persistants permanents et existants. Consultez [liste des Shadows](list-shadows.md) pour la syntaxe et les paramètres.|
|fournisseurs|Répertorie les fournisseurs de clichés instantanés actuellement inscrits. Consultez [répertorier les fournisseurs](list-providers.md) pour la syntaxe et les paramètres.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour répertorier tous les clichés instantanés, tapez :
```
list shadows all
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)