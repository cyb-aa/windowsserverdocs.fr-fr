---
title: set
description: Rubrique de référence pour Set, qui affiche, définit ou supprime CMD. Variables d’environnement EXE.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fdd60d6-addf-4574-8c92-8aa53fa73d76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d537188e1f0fafdd5ecd4075a77397e12328ddf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721883"
---
# <a name="set"></a>set

Affiche, définit ou supprime CMD. Variables d’environnement EXE. En cas d’utilisation sans paramètre, **Set** affiche les paramètres actuels de la variable d’environnement.



## <a name="syntax"></a>Syntaxe

```
set [<Variable>=[<String>]]
set [/p] <Variable>=[<PromptString>]
set /a <Variable>=<Expression>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<> variable|Spécifie la variable d’environnement à définir ou modifier.|
|\<> de chaîne|Spécifie la chaîne à associer à la variable d’environnement spécifiée.|
|/p|Définit la valeur de la *variable* sur une ligne d’entrée entrée par l’utilisateur.|
|\<PromptString>|facultatif. Spécifie un message pour inviter l’utilisateur à entrer des données. Ce paramètre est utilisé avec l’option de ligne de commande **/p** .|
|/a|Définit *String* sur une expression numérique évaluée.|
|\<Expression>|Spécifie une expression numérique. Consultez la section Notes pour connaître les opérateurs valides qui peuvent être utilisés dans *expression*.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

- Utilisation de **Set** avec les extensions de commande activées

  Lorsque les extensions de commande sont activées (valeur par défaut) et que vous exécutez **Set** avec une valeur, elle affiche toutes les variables qui commencent par cette valeur.
- Utilisation de caractères spéciaux

  Les caractères **<**, **>** **|**,, **&**, **^** sont des caractères d’interpréteur de commandes spéciaux qui doivent être précédés du caractère**^** d’échappement () ou placés entre guillemets lorsqu’ils sont utilisés dans une *chaîne* (par exemple, **StringContaining&Symbol**). Si vous utilisez des guillemets pour encadrer une chaîne qui contient l’un des caractères spéciaux, les guillemets sont définis dans le cadre de la valeur de la variable d’environnement.
- Utilisation de variables d’environnement

  Utilisez des variables d’environnement pour contrôler le comportement de certains fichiers et programmes de commandes et pour contrôler la façon dont Windows et le sous-système MS-DOS s’affichent et fonctionnent. La commande **Set** est souvent utilisée dans le fichier Autoexec. NT pour définir des variables d’environnement.
- Affichage des paramètres actuels de l’environnement

  Lorsque vous tapez la commande **Set** uniquement, les paramètres d’environnement actuels sont affichés. Ces paramètres incluent généralement les variables d’environnement COMSPEC et PATH, qui permettent de trouver des programmes sur disque. Les deux autres variables d’environnement utilisées par Windows sont PROMPT et DIRCMD.
- Utilisation des paramètres

  Lorsque vous spécifiez des valeurs pour la *variable* et la *chaîne*, la valeur de la *variable* spécifiée est ajoutée à l’environnement et la *chaîne* est associée à cette variable. Si la variable existe déjà dans l’environnement, la nouvelle valeur de chaîne remplace l’ancienne valeur de chaîne.

  Si vous spécifiez uniquement une variable et un signe égal (sans *chaîne*) pour la commande **Set** , la valeur de *chaîne* associée à la variable est désactivée (comme si la variable était absente).
- Utilisation de **/a**

  Le tableau suivant répertorie les opérateurs pris en charge pour **/a** dans l’ordre de priorité décroissant.  

  |        Opérateur         | Opération effectuée  |
  |-------------------------|----------------------|
  |           ( )           |       Regroupement       |
  |          ! ~ -          |        Unaire         |
  |         \* / %          |      Arithmétique      |
  |           + -           |      Arithmétique      |
  |          << >>          |    Décalage logique     |
  |            &            |     ET au niveau du bit      |
  |            ^            | OU exclusif au niveau du bit |
  |                         |                      |
  | = \*=/=% = + =-= &= ^ = |      = <<= >>=       |
  |            ,            | Séparateur d’expressions |

  Si vous utilisez des opérateurs**&&** logiques (ou **||**)**%** ou modulo (), mettez la chaîne d’expression entre guillemets. Toutes les chaînes non numériques de l’expression sont considérées comme des noms de variables d’environnement et leurs valeurs sont converties en nombres avant d’être traitées. Si vous spécifiez un nom de variable d’environnement qui n’est pas défini dans l’environnement actuel, la valeur zéro est allouée, ce qui vous permet d’effectuer des opérations arithmétiques avec des valeurs de variables d’environnement sans utiliser% pour récupérer une valeur.

  Si vous exécutez **set/a** à partir de la ligne de commande en dehors d’un script de commande, la valeur finale de l’expression est affichée.

  Les valeurs numériques sont des nombres décimaux, sauf si elles sont précédées de 0 × pour les nombres hexadécimaux ou 0 pour les nombres octaux. Par conséquent, 0 × 12 est le même que 18, ce qui est identique à 022.
- Prise en charge du développement variable d’environnement retardé

  La prise en charge de l’expansion des variables d’environnement retardée est désactivée par défaut, mais vous pouvez l’activer ou la désactiver à l’aide de **cmd/v**.
- Utilisation des extensions de commande

  Lorsque les extensions de commande sont activées (valeur par défaut) et que vous exécutez **Set** seul, elle affiche toutes les variables d’environnement actuelles. Si vous exécutez **Set** avec une valeur, il affiche les variables qui correspondent à cette valeur.
- Utilisation de **Set** dans des fichiers de commandes

  Lorsque vous créez des fichiers de commandes, vous pouvez utiliser **Set** pour créer des variables, puis les utiliser de la même façon que vous utilisez les variables numérotées **%0** à **%9**. Vous pouvez également utiliser les variables **%0** à **%9** comme entrée pour **Set**.
- Appel d’une variable **Set** à partir d’un fichier de commandes

  Lorsque vous appelez une valeur de variable à partir d’un fichier de commandes, placez la valeur**%** entre les signes de pourcentage (). Par exemple, si votre programme de traitement par lots crée une variable d’environnement nommée BAUD, vous pouvez utiliser la chaîne associée à BAUD comme paramètre remplaçable en tapant **% Baud%** à l’invite de commandes.
- Utilisation de **Set** sur la console de récupération

  La commande **Set** , avec des paramètres différents, est disponible à partir de la console de récupération.

## <a name="examples"></a>Exemples

Pour définir une variable d’environnement nommée TEST ^ 1, tapez :
```
set testVar=test^^1
```

> [!NOTE]
> La commande **Set** affecte tout ce qui suit le signe égal (=) à la valeur de la variable. Si vous tapez :
> ```
> set testVar=test^1
> ```
> Vous obtenez le résultat suivant :
> ```
> testVar=test^1
> ```
> Pour définir une variable d’environnement nommée TEST&1, tapez :
> ```
> set testVar=test^&1
> ```
> Pour définir une variable d’environnement nommée INCLUDe afin qu’elle soit associée à la chaîne C:\Inc (le répertoire \Inc sur le lecteur C), tapez :
> ```
> set include=c:\inc
> ```
> Vous pouvez ensuite utiliser la chaîne C:\Inc dans les fichiers de commandes en plaçant le nom INCLUDe avec des**%** signes de pourcentage (). Par exemple, vous pouvez inclure la commande suivante dans un fichier de commandes afin d’afficher le contenu du répertoire associé à la variable d’environnement INCLUDe :
> ```
> dir %include%
> ```
> Lorsque cette commande est traitée, la chaîne C:\Inc remplace **% include%**.

Vous pouvez également utiliser **Set** dans un programme de traitement par lots qui ajoute un nouveau répertoire à la variable d’environnement PATH. Par exemple :
```
@echo off
rem ADDPATH.BAT adds a new directory
rem to the path environment variable.
set path=%1;%path%
set
```
Pour afficher la liste de toutes les variables d’environnement qui commencent par la lettre P, tapez :
```
set p 
```

> [!NOTE]
> Cette commande requiert des extensions de commande, qui sont activées par défaut.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)