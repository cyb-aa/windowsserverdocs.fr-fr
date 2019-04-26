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
ms.openlocfilehash: 87b10952c6a851b5536a1589b994b265e8699f59
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826400"
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
Pour exclure un writer portant le nom « enregistreur de système ? Type :
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)