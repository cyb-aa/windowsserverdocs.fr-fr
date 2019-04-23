---
title: jeu
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fdd60d6-addf-4574-8c92-8aa53fa73d76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ece1581bad3d78add93a5bac2c4331ebc5240eef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882130"
---
# <a name="set"></a>jeu



Affiche, définit ou supprime CMD. Variables d’environnement EXE. Si utilisée sans paramètres, **définir** affiche les paramètres de variable d’environnement actuel.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
set [<Variable>=[<String>]]
set [/p] <Variable>=[<PromptString>]
set /a <Variable>=<Expression>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Variable>|Spécifie la variable d’environnement pour définir ou modifier.|
|\<chaîne >|Spécifie la chaîne à associer à la variable d’environnement spécifiée.|
|/p|Définit la valeur de *Variable* à une ligne d’entrée entrée par l’utilisateur.|
|\<PromptString>|Facultatif. Spécifie un message invite l’utilisateur pour l’entrée. Ce paramètre est utilisé avec le **/p** option de ligne de commande.|
|/a|Jeux *chaîne* à une expression numérique qui est évaluée.|
|\<Expression>|Spécifie une expression numérique. Consultez la section Notes pour les opérateurs valides qui peuvent être utilisés dans *Expression*.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   À l’aide de **définir** avec des extensions de commande est activées

    Lorsque les extensions de commande sont activées (valeur par défaut) et que vous exécutez **définir** avec une valeur, elle affiche toutes les variables qui commencent par cette valeur.
-   À l’aide de caractères spéciaux

    Les caractères **<**, **>**, **|**, **&**, **^** sont des caractères spéciaux de commandes shell, et il doivent être précédés par le caractère d’échappement (**^**) ou entre guillemets lorsqu’il est utilisé dans *chaîne* (par exemple, **« Symbole & EstContenuDansChaîne »**). Si vous utilisez des guillemets pour délimiter une chaîne qui contient l’un des caractères spéciaux, les guillemets sont définies en tant que partie de la valeur de variable d’environnement.
-   À l’aide de variables d’environnement

    Utiliser des variables d’environnement pour contrôler le comportement de certains fichiers de commandes et les programmes et pour contrôler la façon dont Windows et le sous-système MS-DOS apparaît et fonctionne. Le **définir** commande est souvent utilisée dans le fichier Autoexec.nt pour définir les variables d’environnement.
-   Afficher les paramètres d’environnement actuel

    Lorsque vous tapez le **définir** commande seul, les paramètres d’environnement en cours sont affichés. Ces paramètres incluent généralement les variables d’environnement COMSPEC et chemin d’accès, qui sont utilisés pour rechercher des programmes sur le disque. Deux autres variables d’environnement utilisées par Windows sont PROMPT et DIRCMD.
-   À l’aide de paramètres

    Lorsque vous spécifiez des valeurs pour *Variable* et *chaîne*, spécifié *variable* valeur est ajoutée à l’environnement et *chaîne* est associé à cette variable. Si la variable existe déjà dans l’environnement, la nouvelle valeur de chaîne remplace l’ancienne valeur de chaîne.

    Si vous ne spécifiez qu’une variable et un signe égal (sans *chaîne*) pour le **définir** commande, le *chaîne* valeur associée à la variable est supprimée (comme si la variable Il y).
-   À l’aide de **/a**

    Le tableau suivant répertorie les opérateurs pris en charge pour **/a** dans l’ordre de priorité décroissant.  
    |Opérateur|Opération effectuée|
    |--------|-------------------|
    |( )|Regroupement|
    |! ~ -|Unaire|
    |* / %|opérations arithmétiques|
    |+ -|opérations arithmétiques|
    |<< >>|Décalage logique|
    |&|AND au niveau du bit|
    |^|Au niveau du bit OR exclusif|
    |||OR au niveau du bit|
    |= *= /= %= += -= &= ^= |= <<= >>=|Assignment|
    |,|Séparateur d’expressions|

    Si vous utilisez logique (**&&** ou **||**) ou de modulo (**%**) opérateurs, placez la chaîne d’expression entre guillemets. Toutes les chaînes non numériques dans l’expression sont considérés comme des noms de variables d’environnement et leurs valeurs sont converties en nombres avant leur traitement. Si vous spécifiez un nom de variable d’environnement qui n’est pas défini dans l’environnement actuel, la valeur zéro est allouée, ce qui vous permet d’effectuer les opérations arithmétiques avec des valeurs de variables d’environnement sans utiliser le % pour récupérer une valeur.

    Si vous exécutez **set /a** à partir de la ligne de commande en dehors d’un script de commande, il affiche la valeur finale de l’expression.

    Valeurs numériques sont des nombres décimaux, sauf si le préfixe par 0 × pour les nombres hexadécimaux ou 0 pour la notation octale. Par conséquent, 0 x 12 est le même que 18, qui est le même que 022.
-   Prise en charge d’expansion de variables d’environnement retardées

    Prise en charge de la variable d’extension environnement différée est désactivée par défaut, mais vous pouvez activer ou désactiver en utilisant **cmd /v**.
-   Utilisation des extensions de commande

    Lorsque les extensions de commande sont activées (valeur par défaut) et que vous exécutez **définir** seul, il affiche toutes les variables d’environnement actuel. Si vous exécutez **définir** avec une valeur, elle affiche les variables qui correspondent à cette valeur.
-   À l’aide de **définir** dans des fichiers batch

    Lorsque vous créez des fichiers de commandes, vous pouvez utiliser **définir** pour créer des variables, puis les utiliser dans la même façon que vous utiliseriez les variables numérotées **%0** via **%9**. Vous pouvez également utiliser les variables **%0** via **%9** comme entrée pour **définir**.
-   Appeler un **définir** variable à partir d’un fichier de commandes

    Lorsque vous appelez une valeur de variable à partir d’un fichier de commandes, placez la valeur entre signes de pourcentage (**%**). Par exemple, si votre programme de traitement par lots crée une variable d’environnement nommée BAUD, vous pouvez utiliser la chaîne associée à BAUD comme un paramètre remplaçable, en tapant **(en bauds) %** à l’invite de commandes.
-   À l’aide de **définir** sur la Console de récupération

    Le **définir** commande, avec des paramètres différents, est disponible à partir de la Console de récupération.

## <a name="BKMK_examples"></a>Exemples

Pour définir une variable d’environnement nommée TEST ^ 1, type :
```
set testVar=test^^1
```

> [!NOTE]
> Le **définir** commande affecte tout ce qui suit le signe égal (=) à la valeur de la variable. Si vous tapez :
```
set testVar="test^1"
```
Vous obtenez le résultat suivant :
```
testVar="test^1"
```
Pour définir une variable d’environnement nommée TEST & 1, tapez :
```
set testVar=test^&1
```
Pour définir une variable d’environnement INCLUDE afin que la chaîne C:\Inc (le répertoire \Inc sur le lecteur C) lui est associée, tapez :
```
set include=c:\inc
```
Vous pouvez ensuite utiliser la chaîne C:\Inc dans des fichiers batch en plaçant le nom INCLUDE entre signes de pourcentage (**%**). Par exemple, vous pouvez inclure la commande suivante dans un fichier de commandes afin que vous pouvez afficher le contenu du répertoire qui est associé à la variable d’environnement INCLUDE :
```
dir %include%
```
Lorsque cette commande est traitée, la chaîne C:\Inc remplace **% incluent %**.

Vous pouvez également utiliser **définir** dans un programme de traitement par lots qui ajoute un nouveau répertoire à la variable d’environnement PATH. Exemple :
```
@echo off
rem ADDPATH.BAT adds a new directory
rem to the path environment variable.
set path=%1;%path%
set
```
Pour afficher une liste de toutes les variables d’environnement qui commencent par la lettre P, tapez :
```
set p 
```

> [!NOTE]
> Cette commande requiert des extensions de commande, qui sont activées par défaut.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)