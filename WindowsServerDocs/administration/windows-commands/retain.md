---
title: maintien
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7391c0b60d07cc2eb5a6230b283ac180cc028508
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722338"
---
# <a name="retain"></a>maintien



Prépare un volume simple dynamique existant à utiliser comme volume de démarrage ou de système.

## <a name="syntax"></a>Syntaxe

```
retain
```

## <a name="remarks"></a>Notes

-   Sur un disque dynamique d’enregistrement de démarrage principal (MBR), cette commande crée une entrée de partition dans l’enregistrement de démarrage principal.
-   Sur un disque dynamique de table de partition GUID (GPT), cette commande crée une entrée de partition dans la table de partition GUID.

## <a name="additional-references"></a>Références supplémentaires

