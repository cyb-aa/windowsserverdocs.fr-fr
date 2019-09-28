---
title: maintien
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b076e12c833645833f53a06476e62bbf44f2690
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384489"
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

#### <a name="additional-references"></a>Références supplémentaires

