---
title: move
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d651e586c31ff64664079bdd10ffde3701ec317d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824990"
---
# <a name="move"></a>move



Déplace un ou plusieurs fichiers d’un répertoire vers un autre répertoire.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
move [{/y | /-y}] [<Source>] [<Target>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/y|Supprime l’invite pour confirmer que vous souhaitez remplacer un fichier de destination existant.|
|/-y|Demande la confirmation que vous souhaitez remplacer un fichier de destination existant.|
|\<Source>|Spécifie le chemin d’accès et le nom du fichier ou des fichiers à déplacer. Si vous souhaitez déplacer ou renommer un répertoire, *Source* doit être le chemin d’accès du répertoire actuel et le nom.|
|\<Target>|Spécifie le chemin d’accès et le nom à déplacer les fichiers vers. Si vous souhaitez déplacer ou renommer un répertoire, *cible* doit contenir le nom et le chemin d’accès du répertoire de votre choix.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Le **/y** option de ligne de commande peut être prédéfinie dans la variable d’environnement COPYCMD. Vous pouvez remplacer cette avec **/-y** sur la ligne de commande. La valeur par défaut consiste à demander confirmation avant de remplacer les fichiers, sauf si le **copie** commande est exécutée à partir d’un script de commande.
-   Déplacement de fichiers cryptés vers un volume qui ne prend pas en charge les résultats du système de fichiers EFS (Encrypting File System) dans une erreur. Tout d’abord de déchiffrer les fichiers ou déplacez les fichiers vers un volume qui prend en charge EFS.

## <a name="BKMK_examples"></a>Exemples

Pour déplacer tous les fichiers portant l’extension .xls à partir du répertoire \Data vers le répertoire \Second_Q\Reports, tapez :
```
move \data\*.xls \second_q\reports\ 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)