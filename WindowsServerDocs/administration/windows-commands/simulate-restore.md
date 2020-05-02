---
title: simuler la restauration
description: Rubrique de référence pour simuler la restauration, qui teste l’implication de l’enregistreur dans les sessions de restauration sur l’ordinateur sans émettre des événements prerestauration ou PostRestore aux rédacteurs.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bab6c56cddc1d2ac95dc70205b0990b82fbfd12
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721778"
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

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)