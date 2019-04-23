---
title: Conserver
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4b437e9f0c8d671e4378311d450aa0ac7639219f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852170"
---
# <a name="retain"></a>Conserver



Prépare un volume simple dynamique existant à utiliser comme un démarrage ou un volume système.

## <a name="syntax"></a>Syntaxe

```
retain
```

## <a name="remarks"></a>Notes

-   Sur un disque dynamique master boot record (MBR), cette commande crée une entrée de partition dans l’enregistrement de démarrage principal.
-   Sur un GUID (GPT) dynamique disque, cette commande crée une entrée de partition dans la table de partition GUID.

#### <a name="additional-references"></a>Références supplémentaires

