---
title: move
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4302a3403f73892500c3f9deb608e9489c7e91ae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373526"
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
|/-y|Invite à confirmer que vous souhaitez remplacer un fichier de destination existant.|
|\<> source|Spécifie le chemin d’accès et le nom du ou des fichiers à déplacer. Si vous souhaitez déplacer ou renommer un répertoire, la *source* doit être le chemin d’accès et le nom du répertoire actif.|
|\<> cible|Spécifie le chemin d’accès et le nom des fichiers à déplacer. Si vous souhaitez déplacer ou renommer un répertoire, *cible* doit être le chemin d’accès au répertoire et le nom souhaités.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   L’option de ligne de commande **/y** peut être prédéfinie dans la variable d’environnement COPYCMD. Vous pouvez le remplacer par **/-y** sur la ligne de commande. La valeur par défaut est l’invite à confirmer le remplacement des fichiers, sauf si la commande de **copie** est exécutée à partir d’un script de commandes.
-   Le déplacement de fichiers chiffrés vers un volume qui ne prend pas en charge système de fichiers EFS (EFS) génère une erreur. Déchiffrez les fichiers en premier ou déplacez les fichiers vers un volume qui prend en charge EFS.

## <a name="BKMK_examples"></a>Illustre

Pour déplacer tous les fichiers avec l’extension. xls du répertoire \Data vers le répertoire \ Second_Q \Rapports, tapez :
```
move \data\*.xls \second_q\reports\ 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)