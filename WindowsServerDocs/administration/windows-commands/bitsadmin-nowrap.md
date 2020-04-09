---
title: bitsadmin nowrap
description: La rubrique commandes Windows pour **Bitsadmin nowrap**, qui tronque toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9f1db370d8a8917aa03a414a27623a1024df192
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850182"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Tronque toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande. Par défaut, tous les commutateurs, à l’exception du commutateur d' **analyse** , encapsulent la sortie. Spécifiez le commutateur **nowrap** avant les autres commutateurs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /nowrap
```

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère l’état de la tâche nommée *myDownloadJob* et n’inclut pas la sortie dans un wrapper.

```
C:\>bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)