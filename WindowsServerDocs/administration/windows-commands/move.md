---
title: déplacer
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1df37753239fea5d5ba9ba22256706a47d4c6a2f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723909"
---
# <a name="move"></a>déplacer



Déplace un ou plusieurs fichiers d’un répertoire vers un autre répertoire.



## <a name="syntax"></a>Syntaxe

```
move [{/y | /-y}] [<Source>] [<Target>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/y|Supprime l’invite pour confirmer que vous souhaitez remplacer un fichier de destination existant.|
|/-y|Invite à confirmer que vous souhaitez remplacer un fichier de destination existant.|
|\<> source|Spécifie le chemin d’accès et le nom du ou des fichiers à déplacer. Si vous souhaitez déplacer ou renommer un répertoire, la *source* doit être le chemin d’accès et le nom du répertoire actif.|
|\<Target>|Spécifie le chemin d’accès et le nom des fichiers à déplacer. Si vous souhaitez déplacer ou renommer un répertoire, *cible* doit être le chemin d’accès au répertoire et le nom souhaités.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   L’option de ligne de commande **/y** peut être prédéfinie dans la variable d’environnement COPYCMD. Vous pouvez le remplacer par **/-y** sur la ligne de commande. La valeur par défaut est l’invite à confirmer le remplacement des fichiers, sauf si la commande de **copie** est exécutée à partir d’un script de commandes.
-   Le déplacement de fichiers chiffrés vers un volume qui ne prend pas en charge système de fichiers EFS (EFS) génère une erreur. Déchiffrez les fichiers en premier ou déplacez les fichiers vers un volume qui prend en charge EFS.

## <a name="examples"></a>Exemples

Pour déplacer tous les fichiers avec l’extension. xls du répertoire \Data vers le répertoire \ Second_Q \Rapports, tapez :
```
move \data\*.xls \second_q\reports\ 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)