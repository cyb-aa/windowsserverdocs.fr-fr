---
title: tree
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22875e63526dc3465021c9aa990f6cea388b81e4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385622"
---
# <a name="tree"></a>tree



Affiche la structure de répertoires d’un chemin d’accès ou du disque d’un lecteur de manière graphique.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
tree [<Drive>:][<Path>] [/f] [/a]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|> @no__t 0Drive :|Spécifie le lecteur qui contient le disque pour lequel vous souhaitez afficher la structure de répertoires.|
|@no__t 0Path >|Spécifie le répertoire pour lequel vous souhaitez afficher la structure de répertoires.|
|/f|Affiche les noms des fichiers dans chaque répertoire.|
|/a|Spécifie que l' **arborescence** doit utiliser des caractères de texte plutôt que des caractères graphiques pour afficher les lignes qui lient des sous-répertoires.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

La structure affichée par l' **arborescence** dépend des paramètres que vous spécifiez à l’invite de commandes. Si vous ne spécifiez pas de lecteur ou de chemin d’accès, l' **arborescence** affiche la structure de l’arborescence en commençant par le répertoire actif du lecteur actif.

## <a name="BKMK_examples"></a>Illustre

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

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)