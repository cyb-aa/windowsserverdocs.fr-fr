---
title: Simuler la restauration
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09b626939c13d4e38a983435b45d8c47ee2b93a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817250"
---
# <a name="simulate-restore"></a>Simuler la restauration



Implication de l’enregistreur des tests dans des sessions de restauration sur l’ordinateur sans émettre de **PreRestore** ou **PostRestore** événements pour les writers.

## <a name="syntax"></a>Syntaxe

```
simulate restore
```

## <a name="remarks"></a>Notes

-   **Simuler la restauration** utilisé pour tester la restauration avec les enregistreurs peut être correctement ou non.
-   Avant de pouvoir utiliser **simuler restauration**, vous devez charger un fichier de métadonnées DiskShadow à l’aide de la **charger les métadonnées** commande. Cela charge les writers sélectionnées et les composants pour la restauration.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)