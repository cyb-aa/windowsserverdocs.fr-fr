---
title: Date
description: Rubrique de référence pour la commande date, qui affiche ou définit la date système. En cas d’utilisation sans paramètre,
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 64d0d94061e1b5c7891b364f4c0fe153b44a564e
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993199"
---
# <a name="date"></a>Date

Affiche ou définit la date système. En cas d’utilisation sans paramètre, **Date** affiche le paramètre de date système actuel et vous invite à entrer une nouvelle date.

>[!IMPORTANT]
> Pour utiliser cette commande, vous devez être administrateur.

## <a name="syntax"></a>Syntaxe

```
date [/t | <month-day-year>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<month-day-year>` | Définit la date spécifiée, où *Month* est le mois (un ou deux chiffres, y compris les valeurs 1 à 12), *Day* est le jour (un ou deux chiffres, y compris les valeurs 1 à 31) et *year* est l’année (deux ou quatre chiffres, y compris les valeurs 00 à 99 ou 1980 à 2099). Vous devez séparer les valeurs du *mois*, du *jour*et de l' *année* par des points (.), des traits d’Union (-) ou des barres obliques (/).<p>**Remarque :** Sachez que si vous utilisez 2 chiffres pour représenter l’année, les valeurs 80-99 correspondent à 1980 à 1999. |
| /t | Affiche la date actuelle sans vous inviter à entrer une nouvelle date. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

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
Enter the new date: (mm-dd-yyyy)
```

Pour conserver la date actuelle et revenir à l’invite de commandes, appuyez sur **entrée**. Pour modifier la date du jour, tapez la nouvelle date, puis appuyez sur **entrée**.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)