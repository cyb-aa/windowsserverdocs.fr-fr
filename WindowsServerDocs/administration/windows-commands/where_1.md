---
title: où
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0b3486a5-896b-4d92-84b8-e463a0b76487
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abebe5799075653d2ace1af4eadbdd5d477d97a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362158"
---
# <a name="where"></a>où



Affiche l’emplacement des fichiers qui correspondent au modèle de recherche donné.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...] 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/r \<Dir >|Indique une recherche récursive, en commençant par le répertoire spécifié.|
|/q|Retourne un code de sortie (**0** pour réussite, **1** pour échec) sans afficher la liste des fichiers correspondants.|
|/f|Affiche les résultats de la commande **Where** entre guillemets.|
|commutateur|Affiche la taille du fichier et la date et l’heure de la dernière modification de chaque fichier correspondant.|
|[$ \<ENV >: \| @ no__t-2Path >:] \<Pattern > [...]|Spécifie le modèle de recherche pour les fichiers à faire correspondre. Au moins un modèle est requis, et le modèle peut inclure des caractères génériques **&#42;** (et **?** ). Par défaut, **où** recherche le répertoire actif et les chemins d’accès spécifiés dans la variable d’environnement PATH. Vous pouvez spécifier un autre chemin d’accès à rechercher en utilisant le format $*env*:*pattern* (où *env* est une variable d’environnement existante contenant un ou plusieurs chemins d’accès) ou à l’aide du format *path*:*pattern* (où *path* est chemin d’accès du répertoire dans lequel vous souhaitez effectuer la recherche. Ces formats facultatifs ne doivent pas être utilisés avec l’option de ligne de commande **/r** .|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Si vous ne spécifiez pas d’extension de nom de fichier, les extensions répertoriées dans la variable d’environnement PATHEXT sont ajoutées au modèle par défaut.
-   **Où** peut exécuter des recherches récursives, afficher des informations de fichier telles que la date ou la taille et accepter des variables d’environnement à la place des chemins sur les ordinateurs locaux.

## <a name="BKMK_examples"></a>Illustre

Pour rechercher tous les fichiers nommés test dans le lecteur C de l’ordinateur actuel et de ses sous-répertoires, tapez :
```
where /r c:\ test 
```
Pour répertorier tous les fichiers du répertoire public, tapez :
```
where $public:*.*
```
Pour rechercher tous les fichiers nommés Notepad dans le lecteur C de l’ordinateur distant, Computer1 et ses sous-répertoires, tapez :
```
where /r \\computer1\c notepad.*
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)