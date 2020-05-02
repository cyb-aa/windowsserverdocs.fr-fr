---
title: time
description: Découvrez comment définir et afficher l’heure système.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e27b260bdaa8896ad3cf0ad58294467bbb63e1c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721363"
---
# <a name="time"></a>time



Affiche ou définit l’heure système. S’il est utilisé sans paramètres, l' **heure** affiche l’heure système actuelle et vous invite à entrer une nouvelle heure.



## <a name="syntax"></a>Syntaxe

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<HH> [ :\<mm> [ :\<SS> [.\< NN>]]] [AM\|PM]|Définit l’heure système sur la nouvelle heure spécifiée, où *hh* est en heures (obligatoire), *mm* est en minutes et *SS* est en secondes. *Nn* peut être utilisé pour spécifier les centièmes de seconde. Si **am** ou **PM** n’est pas spécifié, l' **heure** utilise le format 24 heures par défaut.|
|/t|Affiche l’heure actuelle sans vous demander une nouvelle heure.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   Pour modifier l’heure actuelle, vous devez disposer d’informations d’identification d’administration.
-   Vous devez séparer les valeurs des valeurs *hh*, *mm*et *SS* par des signes deux-points ( :). *SS* et *nn* doivent être séparés par un point (.).
-   Les valeurs *hh* valides sont comprises entre 0 et 24.
-   Les valeurs valides *mm* et *SS* sont comprises entre 0 et 59.

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre

Si les extensions de commande sont activées, pour afficher l’heure système actuelle, tapez :
```
time /t
```
Pour modifier l’heure système actuelle pour 5:30 P.M., tapez l’une des valeurs suivantes :
```
time 17:30:00
time 5:30 pm
```
Pour afficher l’heure système actuelle, puis une invite pour entrer une nouvelle heure, tapez :
```
The current time is: 17:33:31.35
Enter the new time:
```
Pour conserver l’heure actuelle et revenir à l’invite de commandes, appuyez sur entrée. Pour modifier l’heure actuelle, tapez la nouvelle heure, puis appuyez sur entrée.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)