---
title: où
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ff50405dd53ee383abc8e13f67befecf73e37c1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832450"
---
# <a name="where"></a>où



Affiche l’emplacement des fichiers qui correspondent au modèle de recherche donnée.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...] 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/r \<Dir>|Indique une recherche récursive, en commençant par le répertoire spécifié.|
|/q|Retourne un code de sortie (**0** pour les cas de réussite, **1** d’échec) sans afficher la liste des fichiers de mise en correspondance.|
|/f|Affiche les résultats de la **où** commande entre guillemets.|
|/t|Affiche la taille du fichier et la date de dernière modification et l’heure de chaque fichier de mise en correspondance.|
|[$\<ENV > :\|\<chemin d’accès > :]\<modèle > [...]|Spécifie le modèle de recherche pour les fichiers faire correspondre. Au moins un modèle est requis, et le modèle peut inclure des caractères génériques (**&#42;** et **?**). Par défaut, **où** recherche dans le répertoire actif et les chemins d’accès qui sont spécifiés dans la variable d’environnement PATH. Vous pouvez spécifier un autre chemin d’accès pour rechercher en utilisant le format $*ENV*:*modèle* (où *ENV* est une variable d’environnement existant contenant un ou plusieurs chemins d’accès) ou à l’aide de le format *chemin d’accès*:*modèle* (où *chemin d’accès* est le chemin du répertoire à rechercher). Ces formats facultatifs ne doivent pas être utilisés avec le **/r** option de ligne de commande.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Si vous ne spécifiez pas une extension de nom de fichier, les extensions répertoriées dans la variable d’environnement PATHEXT sont ajoutées au modèle par défaut.
-   **Où** peut exécuter des recherches récursives, afficher des informations de fichier tels que la date ou de taille et accepter des variables d’environnement à la place des chemins d’accès sur les ordinateurs locaux.

## <a name="BKMK_examples"></a>Exemples

Pour rechercher tous les fichiers nommés Test dans le lecteur C de l’ordinateur actuel et ses sous-répertoires, tapez :
```
where /r c:\ test 
```
Pour répertorier tous les fichiers dans le répertoire Public, tapez :
```
where $public:*.*
```
Pour rechercher tous les fichiers nommés Bloc-notes sur le lecteur C de l’ordinateur distant, Ordinateur1 et ses sous-répertoires, tapez :
```
where /r \\computer1\c notepad.*
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)