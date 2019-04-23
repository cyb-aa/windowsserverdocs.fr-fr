---
title: date
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e774f7bfabb9b574255691dd97d2cfff36f034e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877950"
---
# <a name="date"></a>date



Affiche ou définit la date système. Si utilisée sans paramètres, **date** affiche la date système actuelle et vous invite à entrer une nouvelle date.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
date [/t | <Month-Day-Year>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Mois-jour-année >|Définit la date spécifiée, où *mois* est le mois (un ou deux chiffres), *jour* est le jour (un ou deux chiffres) et *année* est l’année (deux ou quatre chiffres).|
|/t|Affiche la date actuelle sans vous invite à entrer une nouvelle date.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour modifier la date actuelle, vous devez disposer des informations d’identification administratives.
-   Vous devez séparer les valeurs pour *mois*, *jour*, et *année* par des points (.), des traits d’union (-) ou une barre oblique (/) de marque.
-   Valide *mois* valeurs vont de 1 à 12.
-   Valide *jour* valeurs vont de 1 à 31.
-   Valide *année* valeurs sont agit de 00 à 99 ou 1980 et 2099. Si vous utilisez deux chiffres, les valeurs de 80 à 99 correspondent aux années 1980 et 1999.

## <a name="BKMK_examples"></a>Exemples

Si les extensions de commande sont activées, pour afficher la date système actuelle, tapez :
```
date /t
```
Pour modifier la date système actuelle jusqu’au 3 août 2007, vous pouvez taper les éléments suivants :
```
date 08.03.2007
date 08-03-07
date 8/3/07
```
Pour afficher la date système actuelle, suivie d’une invite à entrer une nouvelle date, tapez :
```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yy)
```
Pour conserver la date actuelle et revenir à l’invite de commandes, appuyez sur ENTRÉE. Pour modifier la date actuelle, tapez la nouvelle date et appuyez sur ENTRÉE.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)