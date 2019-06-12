---
title: enregistreur
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8aee4ecca85c7d5f46ee79f3ad928b746c02e7bb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439986"
---
# <a name="writer"></a>enregistreur



Vérifie qu’un enregistreur ou le composant est inclus ou exclut un enregistreur ou un composant à partir de la procédure de sauvegarde ou de restauration. Si utilisée sans paramètres, **writer** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>Paramètres

| Paramètre  |                                                                                      Description                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | Vérifie que le writer spécifié ou le composant est inclus dans la procédure de sauvegarde ou de restauration. La procédure de sauvegarde ou de restauration échoue si l’enregistreur ou le composant n’est pas inclus. |
|  exclude   |                                                   Exclut le writer spécifié ou le composant à partir de la procédure de sauvegarde ou de restauration.                                                    |
| [\<Writer > |                                                                                     <Component>]                                                                                      |

## <a name="BKMK_examples"></a>Exemples

Pour vérifier un writer en spécifiant son GUID (par exemple, 4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f), tapez :
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Pour exclure un writer portant le nom « System Writer », tapez :
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)