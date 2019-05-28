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
ms.openlocfilehash: 94be02aa25867845436b83d052c4990ff9212975
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564683"
---
# <a name="writer"></a>enregistreur



Vérifie qu’un enregistreur ou le composant est inclus ou exclut un enregistreur ou un composant à partir de la procédure de sauvegarde ou de restauration. Si utilisée sans paramètres, **writer** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|verify|Vérifie que le writer spécifié ou le composant est inclus dans la procédure de sauvegarde ou de restauration. La procédure de sauvegarde ou de restauration échoue si l’enregistreur ou le composant n’est pas inclus.|
|exclude|Exclut le writer spécifié ou le composant à partir de la procédure de sauvegarde ou de restauration.|
|[\<Writer > | <Component>]|Spécifie l’enregistreur ou le composant pour vérifier ou à exclure. Enregistreurs sont spécifiés par le writer GUID ou par le nom du scripteur, par exemple « Writer système ».|

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