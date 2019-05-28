---
title: findstr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea306127be9497c21a5b8efa9fd3f0fa2433014c
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192670"
---
# <a name="findstr"></a>findstr

Rechercher des modèles de texte dans des fichiers.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<File>] [/c:<String>] [/g:<File>] [/d:<DirList>] [/a:<ColorAttribute>] [/off[line]] <Strings> [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/b|Correspond au modèle de texte si elle se trouve au début d’une ligne.|
|/e|Correspond au modèle de texte s’il s’agit à la fin d’une ligne.|
|/l|Processus littéralement les chaînes de recherche.|
|/r|Processus des chaînes de recherche comme des expressions régulières. Il s’agit du paramètre par défaut.|
|/s|Recherche dans le répertoire actif et tous les sous-répertoires.|
|/i|Ignore la casse des caractères lors de la chaîne de recherche.|
|/x|Imprime les lignes qui correspondent exactement.|
|/v|Imprime uniquement les lignes qui ne contiennent pas une correspondance.|
|/n|Imprime le numéro de ligne de chaque ligne qui correspond à.|
|/m|Imprime le nom de fichier uniquement si un fichier contient une correspondance.|
|/o|Offset de caractère imprime avant chaque ligne correspondante.|
|/p|Ignore les fichiers avec des caractères non imprimables.|
|/off[line]|N’ignore pas les fichiers qui ont l’attribut hors connexion.|
|/f :\<fichier >|Obtient une liste de fichiers à partir du fichier spécifié.|
|/ c:\<chaîne >|Utilise le texte spécifié comme une chaîne de recherche littéral.|
|/ g:\<fichier >|Obtient recherche des chaînes à partir du fichier spécifié.|
|/d:\<DirList>|Recherche dans la liste de répertoires spécifiée. Chaque répertoire doit être séparé par un point-virgule ( ;), par exemple `dir1;dir2;dir3`.|
|/ a:\<ColorAttribute >|Spécifie les attributs de couleur avec deux chiffres hexadécimaux. Type `color /?` pour plus d’informations.|
|\<Chaînes >|Spécifie le texte à rechercher dans *FileName*. Obligatoire.|
|[\<Drive>:][<Path>]<FileName>[ ...]|Spécifie l’emplacement et les fichiers à rechercher. Nom d’au moins un fichier est obligatoire.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Tous les **findstr** doivent précéder les options de ligne de commande *chaînes* et *FileName* dans la chaîne de commande.
-   Expressions régulières utilisent les caractères littéraux et les métacaractères pour rechercher des modèles de texte, plutôt que des chaînes exactes de caractères. Un caractère littéral est un caractère qui n’a pas de signification particulière dans la syntaxe d’expression régulière, elle correspond à une occurrence de ce caractère. Par exemple, des lettres et nombres sont des caractères littéraux. Un métacaractère est un symbole avec une signification spéciale (opérateur ou séparateur) dans la syntaxe d’expression régulière.

    Le tableau suivant répertorie les métacaractères qui **findstr** accepte.  
    |Caractère de remplacement|Value|
    |-------------|-----|
    |.|Générique : n’importe quel caractère|
    |*|Répétez : zéro ou plusieurs occurrences de la classe ou le caractère précédent|
    |^|Position de ligne : à compter de la ligne|
    |$|Position de ligne : fin de la ligne|
    |[class]|Classe de caractères : tout caractère dans un jeu|
    |[^class]|Classe inverse : tout caractère non dans un ensemble|
    |[x-y]|Plage : tous les caractères dans la plage spécifiée|
    |\x|D’échappement : utilisation de littéral un métacaractère x|
    |\\<string|Position dans le mot : à compter du mot|
    |Chaîne\>|Position dans le mot : fin du mot|

    Les caractères spéciaux dans la syntaxe d’expression régulière ont le plus de puissance quand vous les utilisez ensemble. Par exemple, utilisez la combinaison suivante du caractère générique (.) et répétez le caractère (*) pour faire correspondre n’importe quelle chaîne de caractères :  
    ```
    .*
    ```  
    Utilisez l’expression suivante dans le cadre d’une expression plus longue pour correspondre à n’importe quelle chaîne commençant par « b » et se terminant par « ing » :  
    ```
    b.*ing
    ```

## <a name="examples"></a>Exemples

Utilisez des espaces pour séparer plusieurs chaînes de recherche, sauf si l’argument est préfixé avec **/c**.

Pour rechercher « hello » ou « there » dans le fichier x.y, tapez :
```
findstr "hello there" x.y 
```
Pour rechercher « Bonjour » dans le fichier x.y, tapez :
```
findstr /c:"hello there" x.y 
```
Pour trouver toutes les occurrences du mot « Windows » (avec une initiale majuscule W) dans le fichier devis.txt, tapez :
```
findstr Windows proposal.txt 
```
Pour rechercher tous les fichiers dans le répertoire actif et tous les sous-répertoires contenant le mot Windows, quelle que soit la casse des lettres, tapez :
```
findstr /s /i Windows *.* 
```
Pour trouver toutes les occurrences des lignes qui commencent par « FOR » et sont précédées par zéro ou plusieurs espaces (comme dans une boucle de programme d’ordinateur) et pour afficher le numéro de ligne où chaque occurrence est trouvée, tapez :
```
findstr /b /n /r /c:"^ *FOR" *.bas 
```
Pour rechercher plusieurs chaînes dans un ensemble de fichiers, créez un fichier texte qui contient chaque critère de recherche sur une ligne distincte. Vous pouvez également répertorier les fichiers que vous souhaitez effectuer une recherche dans un fichier texte. Par exemple, pour utiliser les critères de recherche dans le fichier Stringlist.txt, rechercher les fichiers indiqués dans Filelist.txt, puis stocker les résultats dans le fichier Result.fin, tapez :
```
findstr /g:stringlist.txt /f:filelist.txt > results.out 
```
Pour répertorier tous les fichiers contenant le mot « ordinateur » dans le répertoire actif et tous les sous-répertoires, indépendamment de la casse, tapez :
```
findstr /s /i /m "\<computer\>" *.*
```
Pour répertorier tous les fichiers contenant le mot « ordinateur » et tous les mots qui commencent par « comp », (par exemple, « compliment » et « participer »), type :
```
findstr /s /i /m "\<comp.*" *.*
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)