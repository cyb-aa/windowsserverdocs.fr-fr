---
title: arborescence
description: Rubrique de référence sur l’arborescence, qui affiche la structure de répertoires d’un chemin d’accès, ou du disque d’un lecteur, graphiquement.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94b0429dadc3965c7e41ad5aa881fc902988ec9b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721285"
---
# <a name="tree"></a>arborescence

Affiche la structure de répertoires d’un chemin d’accès ou du disque d’un lecteur de manière graphique.



## <a name="syntax"></a>Syntaxe

```
tree [<Drive>:][<Path>] [/f] [/a]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<> du lecteur :|Spécifie le lecteur qui contient le disque pour lequel vous souhaitez afficher la structure de répertoires.|
|\<Chemin d’accès>|Spécifie le répertoire pour lequel vous souhaitez afficher la structure de répertoires.|
|/f|Affiche les noms des fichiers dans chaque répertoire.|
|/a|Spécifie que l' **arborescence** doit utiliser des caractères de texte plutôt que des caractères graphiques pour afficher les lignes qui lient des sous-répertoires.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

La structure affichée par l' **arborescence** dépend des paramètres que vous spécifiez à l’invite de commandes. Si vous ne spécifiez pas de lecteur ou de chemin d’accès, l' **arborescence** affiche la structure de l’arborescence en commençant par le répertoire actif du lecteur actif.

## <a name="examples"></a>Exemples

Pour afficher les noms de tous les sous-répertoires sur le disque de votre lecteur actuel, tapez :
```
tree \
```
Pour afficher, un écran à la fois, les fichiers de tous les répertoires sur le lecteur C, tapez :
```
tree c:\ /f | more 
```
Pour imprimer une liste de tous les répertoires sur le lecteur C, tapez :
```
tree c:\ /f  prn 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)