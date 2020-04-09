---
title: tree
description: La rubrique commandes Windows pour l’arborescence, qui affiche la structure de répertoires d’un chemin d’accès, ou du disque d’un lecteur, graphiquement.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14b9a4dfd5c84b55a32dbc3f6fd7e8a2cc00c7ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832672"
---
# <a name="tree"></a>tree

Affiche la structure de répertoires d’un chemin d’accès ou du disque d’un lecteur de manière graphique.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
tree [<Drive>:][<Path>] [/f] [/a]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|> du lecteur de \<:|Spécifie le lecteur qui contient le disque pour lequel vous souhaitez afficher la structure de répertoires.|
|Chemin de \<>|Spécifie le répertoire pour lequel vous souhaitez afficher la structure de répertoires.|
|/f|Affiche les noms des fichiers dans chaque répertoire.|
|/a|Spécifie que l' **arborescence** doit utiliser des caractères de texte plutôt que des caractères graphiques pour afficher les lignes qui lient des sous-répertoires.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

La structure affichée par l' **arborescence** dépend des paramètres que vous spécifiez à l’invite de commandes. Si vous ne spécifiez pas de lecteur ou de chemin d’accès, l' **arborescence** affiche la structure de l’arborescence en commençant par le répertoire actif du lecteur actif.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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