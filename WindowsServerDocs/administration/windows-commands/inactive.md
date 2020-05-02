---
title: inactive
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f1c0e0cd5ebbf92638a221852bc3133116f4911
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724831"
---
# <a name="inactive"></a>inactive



Sur les disques de l’enregistrement de démarrage principal (MBR) de base, marque la partition système ou la partition de démarrage qui a le focus comme étant inactive.

## <a name="syntax"></a>Syntaxe

```
inactive
```

## <a name="remarks"></a>Notes

> [!CAUTION]
> Il se peut que votre ordinateur ne démarre pas sans une partition active. Ne Marquez pas une partition système ou de démarrage comme inactive, sauf si vous êtes un utilisateur expérimenté ayant une connaissance approfondie de la famille de systèmes d’exploitation Windows.</br>> si vous ne parvenez pas à démarrer votre ordinateur après avoir marqué le système ou la partition de démarrage comme étant inactif, insérez le CD installation de Windows dans le lecteur de CD-ROM, redémarrez l’ordinateur, puis réparez la partition à l’aide des commandes **FIXMBR** et **FIXBOOT** dans la console de récupération.
> -   Une fois que vous avez marqué la partition système ou la partition de démarrage comme inactive, votre ordinateur démarre à partir de l’option suivante spécifiée dans le BIOS, comme le lecteur de CD-ROM ou un environnement PXE (Pre-Boot eXecution Environment).
> -   Une partition système ou de démarrage active doit être sélectionnée pour que cette opération aboutisse. Utilisez la commande **Sélectionner une partition** pour sélectionner la partition active et y déplacer le focus.

## <a name="examples"></a>Exemples

```
inactive
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

