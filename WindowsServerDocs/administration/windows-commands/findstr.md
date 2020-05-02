---
title: findstr
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97cc58d2b87190c43137e8b193f0217fb98c006c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725601"
---
# <a name="findstr"></a>findstr

Recherche des modèles de texte dans les fichiers.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<File>] [/c:<String>] [/g:<File>] [/d:<DirList>] [/a:<ColorAttribute>] [/off[line]] <Strings> [<Drive>:][<Path>]<FileName>[ ...]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/b|Correspond au modèle de texte s’il se trouve au début d’une ligne.|
|/e|Correspond au modèle de texte s’il se trouve à la fin d’une ligne.|
|/l|Traite les chaînes de recherche littéralement.|
|/r|Traite les chaînes de recherche en tant qu’expressions régulières. Il s'agit du paramètre par défaut.|
|/s|Recherche le répertoire actif et tous ses sous-répertoires.|
|/i|Ignore la casse des caractères lors de la recherche de la chaîne.|
|/x|Imprime les lignes qui correspondent exactement.|
|/v|Imprime uniquement les lignes qui ne contiennent pas de correspondance.|
|/n|Imprime le numéro de ligne de chaque ligne correspondant à.|
|/m|Imprime uniquement le nom du fichier si un fichier contient une correspondance.|
|/o|Imprime le décalage de caractère avant chaque ligne correspondante.|
|/p|Ignore les fichiers avec des caractères non imprimables.|
|/OFF [ligne]|N’ignore pas les fichiers dont l’attribut offline est défini.|
|/f :\<> de fichiers|Obtient une liste de fichiers à partir du fichier spécifié.|
|/c :\<String>|Utilise le texte spécifié comme chaîne de recherche littérale.|
|/g :\<fichier>|Obtient les chaînes de recherche à partir du fichier spécifié.|
|/d :\<DirList>|Recherche la liste de répertoires spécifiée. Chaque répertoire doit être séparé par un point-virgule (;), `dir1;dir2;dir3`par exemple.|
|/a :\<ColorAttribute>|Spécifie des attributs de couleur avec deux chiffres hexadécimaux. Pour `color /?` plus d’informations, tapez.|
|\<Chaînes>|Spécifie le texte à rechercher dans *filename*. Obligatoire.|
|[\<Lecteur> :] [<Path>]<FileName>[ ...]|Spécifie l’emplacement et les fichiers à rechercher. Au moins un nom de fichier est requis.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

- Toutes les options de ligne de commande **findstr** doivent précéder les *chaînes* et le *nom de fichier* dans la chaîne de commande.
- Les expressions régulières utilisent à la fois des caractères littéraux et des métacaractères pour rechercher des modèles de texte, plutôt que des chaînes exactes de caractères. Un caractère littéral est un caractère qui n’a pas de signification particulière dans la syntaxe d’expression régulière ; il correspond à une occurrence de ce caractère. Par exemple, les lettres et les nombres sont des caractères littéraux. Un métacaractère est un symbole avec une signification spéciale (un opérateur ou un délimiteur) dans la syntaxe d’expression régulière.

  Le tableau suivant répertorie les métacaractères que **findstr** accepte.  

  |Métacaractère|Valeur|
  |-------------|-----|
  |.|Caractère générique : tout caractère|
  |*|REPEAT : zéro, une ou plusieurs occurrences du caractère ou de la classe précédente|
  |^|Position de la ligne : début de la ligne|
  |$|Position de la ligne : fin de la ligne|
  |type|Classe de caractères : n’importe quel caractère d’un jeu|
  |[^ classe]|Classe inverse : tout caractère ne figurant pas dans un jeu|
  |[x-y]|Plage : tous les caractères de la plage spécifiée|
  |\x|Escape : utilisation littérale d’un métacaractère x|
  |\\Chaîne<|Position du mot : début du mot|
  |string\>|Position du mot : fin du mot|

  Les caractères spéciaux de la syntaxe des expressions régulières sont le plus puissant lorsque vous les utilisez ensemble. Par exemple, utilisez la combinaison suivante des caractères génériques (.) et REPEAT (*) pour faire correspondre n’importe quelle chaîne de caractères :

  ```
  .*
  ``` 

  Utilisez l’expression suivante dans le cadre d’une expression plus grande pour faire correspondre n’importe quelle chaîne commençant par b et se terminant par ING : 

  ```
  b.*ing
  ```

## <a name="examples"></a>Exemples

Utilisez des espaces pour séparer plusieurs chaînes de recherche, sauf si l’argument est préfixé avec **/c**.

Pour rechercher Hello ou dans le fichier x. y, tapez :

```
findstr hello there x.y 
```

Pour rechercher « Bonjour » dans le fichier x. y, tapez :

```
findstr /c:hello there x.y 
```

Pour rechercher toutes les occurrences du mot Windows (avec une lettre majuscule initiale W) dans le fichier proposition. txt, tapez :

```
findstr Windows proposal.txt 
```

Pour rechercher dans tous les fichiers du répertoire actif et dans tous les sous-répertoires qui contenaient le mot Windows, quelle que soit la casse, tapez :

```
findstr /s /i Windows *.* 
```

Pour rechercher toutes les occurrences de lignes qui commencent par pour et sont précédées de zéro ou de plusieurs espaces (comme dans une boucle de programme informatique) et pour afficher le numéro de ligne où chaque occurrence est trouvée, tapez :

```
findstr /b /n /r /c:^ *FOR *.bas 
```

Pour rechercher plusieurs chaînes dans un ensemble de fichiers, créez un fichier texte qui contient chaque critère de recherche sur une ligne distincte. Vous pouvez également répertorier les fichiers exacts que vous souhaitez rechercher dans un fichier texte. Par exemple, pour utiliser les critères de recherche dans le fichier StringList. txt, recherchez les fichiers listés dans filelist. txt, puis stockez les résultats dans le fichier results. out, tapez :

```
findstr /g:stringlist.txt /f:filelist.txt > results.out 
```

Pour répertorier chaque fichier contenant le mot ordinateur dans le répertoire actif et tous ses sous-répertoires, quel que soit le cas, tapez :

```
findstr /s /i /m \<computer\> *.*
```

Pour répertorier chaque fichier contenant le mot ordinateur et tous les autres mots qui commencent par COMP, (par exemple compliment et en concurrence), tapez :

```
findstr /s /i /m \<comp.* *.*
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)