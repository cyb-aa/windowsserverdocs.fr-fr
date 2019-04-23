---
title: time
description: Découvrez comment définir et afficher l’heure système.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5c1f43be98a19c4b150c247cc7fd48d62edeb5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861910"
---
# <a name="time"></a>time



Affiche ou définit l’heure système. Si utilisée sans paramètres, **temps** affiche l’heure système actuelle et vous invite à entrer une nouvelle heure.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<HH>[:\<MM>[:\<SS>[.\<NN>]]] [am\|pm]|Définit l’heure système sur la nouvelle heure spécifiée, où *HH* est exprimé en heures (obligatoire), *MM* en minutes, et *SS* est exprimé en secondes. *NN* peut être utilisé pour spécifier les centièmes de seconde. Si **suis** ou **pm** n’est pas spécifié, **temps** utilise le format de 24 heures par défaut.|
|/t|Affiche l’heure actuelle sans vous invite à entrer une nouvelle heure.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour modifier l’heure actuelle, vous devez disposer des informations d’identification administratives.
-   Vous devez séparer les valeurs pour *HH*, *MM*, et *SS* avec des deux-points ( :)). *SS* et *NN* doivent être séparés par un point (.).
-   Valide *HH* valeurs vont de 0 à 24.
-   Valide *MM* et *SS* valeurs vont de 0 à 59.

## <a name="BKMK_examples"></a>Exemples

Si les extensions de commande sont activées, pour afficher l’heure système actuelle, tapez :
```
time /t
```
Pour modifier l’heure système actuelle à 17 h 30, tapez une des opérations suivantes :
```
time 17:30:00
time 5:30 pm
```
Pour afficher l’heure système actuelle, suivie d’une invite à entrer une nouvelle heure, tapez :
```
The current time is: 17:33:31.35
Enter the new time:
```
Pour conserver l’heure actuelle et revenir à l’invite de commandes, appuyez sur ENTRÉE. Pour modifier l’heure actuelle, tapez la nouvelle heure et appuyez sur ENTRÉE.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)