---
title: bitsadmin nowrap
description: La rubrique relative aux commandes Windows pour **Bitsadmin nowrap** -tronque toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3806ec51161eeae498e3c9b367b2aacf0bd32c99
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381053"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Tronque toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /NoWrap
```

## <a name="remarks"></a>Notes

Par défaut, tous les commutateurs, à l’exception du commutateur d' **analyse** , encapsulent la sortie. Spécifiez le commutateur **nowrap** avant les autres commutateurs.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère l’état de la tâche nommée *myDownloadJob* et n’encapsule pas la sortie
```
C:\>bitsadmin /NoWrap /GetState myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)