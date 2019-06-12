---
title: inactif
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9c8ded732d984830c7892720f75938979f1abb67
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438164"
---
# <a name="inactive"></a>inactif



Sur la base de démarrage principal disques MBR (enregistrement), marque la partition système ou une partition de démarrage avec le focus comme étant inactive.

## <a name="syntax"></a>Syntaxe

```
inactive
```

## <a name="remarks"></a>Notes

> [!CAUTION]
> Votre ordinateur ne peut pas démarrer sans une partition active. Ne marquez pas une partition système ou de démarrage comme étant inactive, sauf si vous êtes un utilisateur expérimenté avec une connaissance approfondie de la famille Windows de systèmes d’exploitation.</br>> Si vous ne parvenez pas à démarrer votre ordinateur après avoir marqué la partition système ou de démarrage comme étant inactive, insérez le CD d’installation de Windows dans le lecteur de CD-ROM, redémarrez l’ordinateur, puis réparez la partition à l’aide de la **fixmbr** et **fixboot** commandes dans la Console de récupération.
> -   Une fois que vous marquez la partition système ou une partition de démarrage comme étant inactive, votre ordinateur démarre à partir de l’option suivante spécifiée dans le BIOS, telles que le lecteur de CD-ROM ou une Pre-Boot eXecution Environment (PXE).
> -   Une partition système ou de démarrage active doit être sélectionnée pour cette opération réussisse. Utilisez le **sélectionnez partition** commande pour sélectionner la partition active et déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

```
inactive
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

