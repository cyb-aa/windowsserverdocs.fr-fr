---
title: répertorier les fournisseurs
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 844b4036-c0b9-449d-8347-7d58ef9bf16d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 761099e3b399aeb9e6a3fe1ddd53ed1a667a4ccb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724500"
---
# <a name="list-providers"></a>répertorier les fournisseurs



Répertorie les fournisseurs de clichés instantanés qui sont actuellement inscrits sur le système.



## <a name="syntax"></a>Syntaxe

```
list providers
```

## <a name="examples"></a>Exemples

Pour répertorier les fournisseurs de clichés instantanés actuellement inscrits, tapez :
```
list providers
```
Une sortie semblable à la suivante s’affiche :
```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)