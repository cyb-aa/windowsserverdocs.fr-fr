---
title: date
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d7328b2b5d3c78fdfd741918d76e26195f0af4a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378816"
---
# <a name="date"></a>date



Affiche ou définit la date système. En cas d’utilisation sans paramètre, **Date** affiche le paramètre de date système actuel et vous invite à entrer une nouvelle date.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
date [/t | <Month-Day-Year>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Month-Day-Year >|Définit la date spécifiée, où *Month* est le mois (un ou deux chiffres), *Day* est le jour (un ou deux chiffres) et *year* est l’année (deux ou quatre chiffres).|
|commutateur|Affiche la date actuelle sans vous inviter à entrer une nouvelle date.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour modifier la date actuelle, vous devez disposer d’informations d’identification d’administration.
-   Vous devez séparer les valeurs du *mois*, du *jour*et de l' *année* par des points (.), des traits d’Union (-) ou des barres obliques (/).
-   Les valeurs de *mois* valides sont comprises entre 1 et 12.
-   Les valeurs de *jour* valides sont comprises entre 1 et 31.
-   Les valeurs valides pour l' *année* sont 00 à 99 ou 1980 à 2099. Si vous utilisez deux chiffres, les valeurs 80 à 99 correspondent aux années 1980 à 1999.

## <a name="BKMK_examples"></a>Illustre

Si les extensions de commande sont activées, pour afficher la date système actuelle, tapez :
```
date /t
```
Pour remplacer la date système actuelle par le 3 août 2007, vous pouvez taper l’un des éléments suivants :
```
date 08.03.2007
date 08-03-07
date 8/3/07
```
Pour afficher la date système actuelle, puis une invite pour entrer une nouvelle date, tapez :
```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yy)
```
Pour conserver la date actuelle et revenir à l’invite de commandes, appuyez sur entrée. Pour modifier la date du jour, tapez la nouvelle date, puis appuyez sur entrée.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)