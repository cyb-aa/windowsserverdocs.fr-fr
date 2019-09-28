---
title: Simuler la restauration
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d6652fea4e74c706fcc03b8a547fab771a7c0191
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370882"
---
# <a name="simulate-restore"></a>Simuler la restauration



Teste l’implication de l’enregistreur dans les sessions de restauration sur l’ordinateur sans émettre des événements **prerestauration** ou **postRestore** aux rédacteurs.

## <a name="syntax"></a>Syntaxe

```
simulate restore
```

## <a name="remarks"></a>Notes

-   **Simuler Restore** permet de déterminer si la restauration avec des enregistreurs peut aboutir.
-   Avant de pouvoir utiliser **simuler la restauration**, vous devez charger un fichier de métadonnées DiskShadow à l’aide de la commande **charger les métadonnées** . Cela charge les enregistreurs et les composants sélectionnés pour la restauration.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)