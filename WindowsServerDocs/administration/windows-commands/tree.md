---
title: tree
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: de22de1c9d62ba79c1aa68248109cca88009703a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872630"
---
# <a name="tree"></a>tree



Affiche la structure de répertoires d’un chemin d’accès ou du disque dans un lecteur sous forme graphique.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
tree [<Drive>:][<Path>] [/f] [/a]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Lecteur > :|Spécifie le lecteur qui contient le disque pour lequel vous souhaitez afficher la structure de répertoires.|
|\<Path>|Spécifie le répertoire pour lequel vous souhaitez afficher la structure de répertoires.|
|/f|Affiche les noms des fichiers dans chaque répertoire.|
|/a|Spécifie que **arborescence** consiste à utiliser des caractères de texte au lieu de caractères graphiques pour afficher les lignes qui lient les sous-répertoires.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

La structure affichée par **arborescence** dépend des paramètres que vous spécifiez à l’invite de commandes. Si vous ne spécifiez pas un lecteur ou un chemin d’accès, **arborescence** affiche l’arborescence du répertoire en cours du lecteur actif.

## <a name="BKMK_examples"></a>Exemples

Pour afficher les noms de tous les sous-répertoires sur le disque dans votre lecteur en cours, tapez :
```
tree \
```
Pour afficher, un écran à la fois, les fichiers dans tous les répertoires sur le lecteur C, tapez :
```
tree c:\ /f | more 
```
Pour imprimer une liste de tous les répertoires sur le lecteur C, tapez :
```
tree c:\ /f  prn 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)