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
ms.openlocfilehash: 5b6efff57a512a2e0519b3294c51c073f1f44d8d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832832"
---
# <a name="time"></a>time



Affiche ou définit l’heure système. S’il est utilisé sans paramètres, l' **heure** affiche l’heure système actuelle et vous invite à entrer une nouvelle heure.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<HH > [ :\<MM > [ :\<SS > [.\<NN >]]] [AM\|PM]|Définit l’heure système sur la nouvelle heure spécifiée, où *hh* est en heures (obligatoire), *mm* est en minutes et *SS* est en secondes. *Nn* peut être utilisé pour spécifier les centièmes de seconde. Si **am** ou **PM** n’est pas spécifié, l' **heure** utilise le format 24 heures par défaut.|
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