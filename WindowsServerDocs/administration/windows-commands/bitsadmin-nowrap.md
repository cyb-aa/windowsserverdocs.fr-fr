---
title: bitsadmin nowrap
description: Rubrique de référence pour la commande Bitsadmin nowrap, qui tronque toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2aac604ec3e13026e322d7cb7a9364df46266a0c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717332"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Tronque toute ligne de texte de sortie qui s’étend au-delà du bord le plus à droite de la fenêtre de commande. Par défaut, tous les commutateurs, à l’exception du commutateur d' **analyse** , encapsulent la sortie. Spécifiez le commutateur **nowrap** avant les autres commutateurs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /nowrap
```

## <a name="examples"></a>Exemples

Pour récupérer l’état de la tâche nommée *myDownloadJob* sans encapsuler la sortie :

```
bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
