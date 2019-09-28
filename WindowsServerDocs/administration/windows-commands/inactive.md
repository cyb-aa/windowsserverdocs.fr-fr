---
title: Inactive
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ce91b6a024c165e3aa63148b9ad6dfcc4db7a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375372"
---
# <a name="inactive"></a>Inactive



Sur les disques de l’enregistrement de démarrage principal (MBR) de base, marque la partition système ou la partition de démarrage qui a le focus comme étant inactive.

## <a name="syntax"></a>Syntaxe

```
inactive
```

## <a name="remarks"></a>Notes

> [!CAUTION]
> Il se peut que votre ordinateur ne démarre pas sans une partition active. Ne Marquez pas une partition système ou de démarrage comme inactive, sauf si vous êtes un utilisateur expérimenté ayant une connaissance approfondie de la famille de systèmes d’exploitation Windows.</br>> Si vous ne parvenez pas à démarrer votre ordinateur après avoir marqué le système ou la partition de démarrage comme étant inactif, insérez le CD installation de Windows dans le lecteur de CD-ROM, redémarrez l’ordinateur, puis réparez la partition à l’aide des commandes **FIXMBR** et **FIXBOOT** dans le Console de récupération.
> -   Une fois que vous avez marqué la partition système ou la partition de démarrage comme inactive, votre ordinateur démarre à partir de l’option suivante spécifiée dans le BIOS, comme le lecteur de CD-ROM ou un environnement PXE (Pre-Boot eXecution Environment).
> -   Une partition système ou de démarrage active doit être sélectionnée pour que cette opération aboutisse. Utilisez la commande **Sélectionner une partition** pour sélectionner la partition active et y déplacer le focus.

## <a name="BKMK_examples"></a>Illustre

```
inactive
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

