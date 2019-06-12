---
title: pour
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e275726c-035f-4a74-8062-013c37f5ded1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8764887794857ae56b7c1a3bda656ece18c117f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439211"
---
# <a name="for"></a>pour



Exécute une commande pour chaque fichier dans un ensemble de fichiers.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|{%%\|%}\<Variable>|Obligatoire. Représente un paramètre remplaçable. Utilisez un signe de pourcentage unique ( **%** ) pour mener à bien le **pour** à l’invite de commande. Utiliser des signes de pourcentage double ( **%%** ) pour mener à bien le **pour** commande au sein d’un fichier de commandes. Variables respectent la casse, et ils doivent être représentées comme avec une valeur alphabétique **%A**, **%B**, ou **%C**.|
|(\<Set>)|Obligatoire. Spécifie un ou plusieurs fichiers, répertoires, ou des chaînes de texte ou une plage de valeurs sur lequel exécuter la commande. Les parenthèses sont obligatoires.|
|\<Command>|Obligatoire. Spécifie la commande que vous souhaitez exécuter arrière sur chaque fichier, répertoire ou chaîne de texte, ou sur la plage de valeurs comprises dans *définir*.|
|\<CommandLineOptions>|Spécifie les options de ligne de commande que vous souhaitez utiliser avec la commande spécifiée.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- À l’aide de **pour**

  Vous pouvez utiliser la **pour** commande au sein d’un fichier de commandes ou directement à partir de l’invite de commandes.
- À l’aide des paramètres de lot

  Les attributs suivants s’appliquent à la **pour** commande :  
  - Le **pour** commande remplace **%** <em>Variable</em> ou **%%** <em>Variable</em>par chaque chaîne de texte dans le jeu spécifié jusqu'à ce que la commande spécifiée traite tous les fichiers.
  - Les noms de variables sont respecte la casse, global et pas plus de 52 peut être active à la fois.
  - Pour éviter toute confusion avec les paramètres de lot **%0** via **%9**, vous pouvez utiliser n’importe quel caractère pour *Variable* sauf les chiffres de 0 à 9. Pour simples fichiers de commandes, un seul caractère comme **%Value%** fonctionnera.
  - Vous pouvez utiliser plusieurs valeurs pour *Variable* dans les fichiers de commandes complexes pour distinguer les différentes variables remplaçables.
- Spécification d’un groupe de fichiers

  Le *définir* paramètre peut représenter un seul groupe de fichiers ou de plusieurs groupes de fichiers. Vous pouvez utiliser des caractères génériques ( **&#42;** et **?** ) pour spécifier un fichier de jeu. Ensembles de fichiers valides sont les suivantes :  
  ```
  (*.doc) 
  (*.doc *.txt *.me)
  (jan*.doc jan*.rpt feb*.doc feb*.rpt)
  (ar??1991.* ap??1991.*)
  ```  
  Lorsque vous utilisez le **pour** commande, la première valeur dans *définir* remplace **%** <em>Variable</em> ou **%%** <em>Variable</em>, puis la commande spécifiée traite cette valeur. Ce processus se poursuit jusqu'à ce que tous les fichiers (ou les groupes de fichiers) qui correspondent à la *définir* valeur sont traités.
- À l’aide de la **dans** et **faire** mots clés

  **Dans** et **faire** ne sont pas des paramètres, mais vous devez les utiliser avec **pour**. Si vous omettez un de ces mots clés, un message d’erreur s’affiche.
- À l’aide de formes supplémentaires de **pour**

  Si les extensions de commande sont activées (c'est-à-dire la valeur par défaut), les formes supplémentaires suivantes de **pour** sont pris en charge :  
  - Répertoires uniquement

    Si *définir* contient des caractères génériques ( **&#42;** ou **?** ), le texte spécifié *commande* s’exécute pour chaque répertoire (au lieu d’un ensemble des fichiers dans un répertoire spécifié) qui correspond à *définir*.

    La syntaxe est :  
    ```
    for /d {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>] 
    ```  
  - Récursive

    Parcourt l’arborescence du répertoire qui est enraciné à *lecteur*:*chemin d’accès* et exécute le **pour** instruction dans chaque répertoire de l’arborescence. Si aucun répertoire n’est spécifié après **/r**, le répertoire actif est utilisé comme répertoire racine. Si *définir* est qu’un seul point (.), elle énumère uniquement l’arborescence de répertoires.

    La syntaxe est :  
    ```
    for /r [[<Drive>:]<Path>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    ```  
  - Itération d’une plage de valeurs

    Utilisez une variable itérative pour définir la valeur de départ (*Démarrer*#) et puis parcourir une plage définie de valeurs jusqu'à ce que la valeur dépasse la valeur de fin définie (*fin*#). **/ l** exécute l’itération en comparant *Démarrer*# avec *fin*#. Si *Démarrer*# est inférieure à *fin*# la commande s’exécute. Lorsque la variable itérative dépasse *fin*#, l’interface de commande quitte la boucle. Vous pouvez également utiliser une valeur négative *étape*# pour parcourir une plage de valeurs décroissantes. Par exemple, (1,1,5) génère la séquence 1 2 3 4 5 et (5,-1,1) génère la séquence 1 de 2 3 5 4.

    La syntaxe est :  
    ```
    for /l {%%|%}<Variable> in (<Start#>,<Step#>,<End#>) do <Command> [<CommandLineOptions>]
    ```  
  - Itération et l’analyse du fichier

    Utilisez à la sortie de commande de processus, des chaînes et contenu du fichier d’analyse de fichier.  Utiliser des variables d’itération pour définir le contenu ou les chaînes que vous souhaitez examiner et utiliser les différentes *MotsClésAnalyse* options pour affiner l’analyse.  Utilisez le *MotsClésAnalyse* option pour spécifier quels jetons doivent être passés en tant que variables itératifs de jeton. Notez que lorsqu’il est utilisé sans l’option de jeton, **/f** examine uniquement le premier jeton.

    L’analyse du fichier se compose de lecture de la sortie, la chaîne ou le contenu du fichier, puis avec rupture dans des lignes de texte et l’analyse de chaque ligne en zéro ou plusieurs jetons. Le **pour** boucle est ensuite appelée avec la valeur de la variable itérative définie sur le jeton. Par défaut, **/f** passe la première vide séparées par des jetons à partir de chaque ligne de chaque fichier. Lignes vides sont ignorés.

    Les syntaxes sont :  
    ```
    for /f ["<ParsingKeywords>"] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ("<LiteralString>") do <Command> [<CommandLineOptions>]
    for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ('<Command>') do <Command> [<CommandLineOptions>]
    ```  
    Le *définir* argument spécifie un ou plusieurs noms de fichier. Chaque fichier est ouvert, lire et traité avant de passer au fichier suivant dans *définir*. Pour remplacer la valeur par défaut, l’analyse du comportement, spécifiez *MotsClésAnalyse*. Il s’agit d’une chaîne entre guillemets qui contient un ou plusieurs des mots clés pour spécifier différentes options d’analyse.

    Si vous utilisez le **usebackq** option, utilisez une des syntaxes suivantes :  
    ```
    for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ("<Set>") do <Command> [<CommandLineOptions>]
    for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ('<LiteralString>') do <Command> [<CommandLineOptions>]
    for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in (`<Command>`) do <Command> [<CommandLineOptions>]
    ```  
    Le tableau suivant répertorie les mots clés d’analyse que vous pouvez utiliser pour *MotsClésAnalyse*.  

    |      Mot-clé      |                                                                                                                                                                                                          Description                                                                                                                                                                                                          |
    |-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |     eol=\<c>      |                                                                                                                                                                                   Spécifie un caractère de fin de ligne (juste un caractère).                                                                                                                                                                                    |
    |     Ignorer =\<N >     |                                                                                                                                                                              Spécifie le nombre de lignes à ignorer au début du fichier.                                                                                                                                                                              |
    |   delims=\<xxx>   |                                                                                                                                                                     Spécifie un ensemble de délimiteurs. Cela remplace l’ensemble de délimiteurs par défaut des espaces et de tabulations.                                                                                                                                                                      |
    | jetons =\<X, Y, M : N > | Spécifie les jetons de chaque ligne doivent être passés à la **pour** boucle pour chaque itération. Par conséquent, les noms de variables supplémentaires sont allouées. *M*–*N* spécifie une plage, à partir de la *M*ième le *N*ième jetons. Si le dernier caractère de la **jetons =** chaîne est un astérisque ( **&#42;** ), une variable supplémentaire est allouée, et qu’il reçoit le reste du texte sur la ligne après le dernier jeton qui est analysé. |
    |     usebackq      |                                                                                             Spécifie à : exécuter une chaîne entre guillemets l’arrière en tant que commande, utilisez une chaîne entre guillemets simples comme une chaîne littérale ou, pour les noms de fichiers longs qui contiennent des espaces, autoriser les noms de fichiers dans  *\<définir\>* , chacune être placé entre des guillemets doubles.                                                                                              |


  - Substitution de variable

    Le tableau suivant répertorie la syntaxe facultatifs (pour n’importe quelle variable **je**).  

    |Variable avec le modificateur|Description|
    |----------------------|-----------|
    |%~I|Se développe **%I** qui supprime les guillemets (« »).|
    |%~fI|Se développe **%I** à un nom de chemin d’accès qualifié complet.|
    |%~dI|Se développe **%I** uniquement une lettre de lecteur.|
    |%~pI|Se développe **%I** uniquement un chemin d’accès.|
    |%~nI|Se développe **%I** à un nom de fichier uniquement.|
    |%~xI|Se développe **%I** à une extension de nom de fichier uniquement.|
    |%~sI|Développe le chemin d’accès pour qu’il contienne uniquement les noms courts.|
    |%~aI|Se développe **%I** sur les attributs de fichier du fichier.|
    |%~tI|Se développe **%I** à la date et l’heure du fichier.|
    |%~zI|Se développe **%I** à la taille du fichier.|
    |%~$PATH:I|Recherche les répertoires énumérés dans la variable d’environnement PATH et développe **%I** pour le nom qualifié complet du premier répertoire trouvé. Si le nom de variable d’environnement n’est pas défini ou le fichier n’est pas trouvé par la recherche, ce modificateur se développe en une chaîne vide.|

    Le tableau suivant répertorie les combinaisons de modificateurs que vous pouvez utiliser pour obtenir des résultats composés.  

    |Variable avec modificateurs combinés|Description|
    |--------------------------------|-----------|
    |%~dpI|Se développe **%I** à une lettre de lecteur et le chemin d’accès uniquement.|
    |%~nxI|Se développe **%I** à un nom de fichier et l’extension uniquement.|
    |%~fsI|Se développe **%I** à un nom de chemin d’accès complet avec des noms courts uniquement.|
    |%~dp$PATH:I|Recherche dans les répertoires qui sont répertoriés dans la variable d’environnement PATH pour **%I** et se développe jusqu'à la lettre de lecteur et le chemin d’accès de la première occurrence trouvée.|
    |%~ftzaI|Se développe **%I** à une ligne de sortie est similaire à **dir**.|

    Dans les exemples ci-dessus, vous pouvez remplacer **%I** et chemin d’accès avec les autres valeurs valides. Valide **pour** nom de la variable se termine le **%~** syntaxe.

    En utilisant des noms de variable de majuscule comme **%I**, vous pouvez rendre votre code plus lisible et éviter toute confusion avec les modificateurs, qui ne sont pas sensible à la casse.
- L’analyse d’une chaîne

  Vous pouvez utiliser la **pour /f** logique sur une chaîne immédiate d’analyse en encapsulant *\<LiteralString\>* dans soit : guillemets doubles (*sans* » usebackq ») ou guillemets simples (*avec* « usebackq »)--par exemple, (« MyString ») ou (« MyString »). *\<LiteralString\>*  est traitée comme une seule ligne d’entrée à partir d’un fichier. Lors de l’analyse *\<LiteralString\>* dans des guillemets doubles, les symboles de commandes (telles que **\\ \& \| \> \< \^** ) sont traités comme des caractères ordinaires.
- Analyse de la sortie

  Vous pouvez utiliser la **pour /f** commande pour analyser la sortie d’une commande en plaçant entre guillemets un back *\<commande\>* entre parenthèses. Il est traité comme une ligne de commande, qui est transmise à un enfant de Cmd.exe. La sortie est capturée en mémoire et analysée comme s’il s’agit d’un fichier.

## <a name="BKMK_examples"></a>Exemples

Pour utiliser **pour** dans un fichier de commandes, utilisez la syntaxe suivante :
```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```
Pour afficher le contenu de tous les fichiers dans le répertoire actif ayant l’extension .doc ou .txt, à l’aide de la variable remplaçable **%f**, type :
```
for %f in (*.doc *.txt) do type %f 
```
Dans l’exemple précédent, chaque fichier ayant l’extension .doc ou .txt dans le répertoire actif est remplacé par le **%f** variable jusqu'à ce que le contenu de chaque fichier est affiché. Pour utiliser cette commande dans un fichier de commandes, remplacez chaque occurrence de **%f** avec **%Value%** . Sinon, la variable est ignorée et un message d’erreur s’affiche.

Pour analyser un fichier, en ignorant les lignes de commentaires, type :
```
for /f "eol=; tokens=2,3* delims=," %i in (myfile.txt) do @echo %i %j %k
```
Cette commande analyse chaque ligne Myfile.txt. Il ignore les lignes qui commencent par un point-virgule et transmet le jeton de deuxième et troisième à partir de chaque ligne à la **pour** corps (les jetons sont délimités par des virgules ou des espaces). Le corps de la **pour** références de l’instruction **%i** pour obtenir un jeton, le deuxième **%j** pour obtenir le jeton de tiers, et **%k** pour obtenir tous les autres jetons. Si les noms de fichiers que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte (par exemple, « nom de fichier »). Pour utiliser des guillemets, vous devez utiliser **usebackq**. Sinon, les guillemets sont interprétés en tant que la définition d’une chaîne littérale à analyser.

**%i** est déclaré explicitement dans le **pour** instruction. **%j** et **%k** sont implicitement déclaré à l’aide de **jetons =** . Vous pouvez utiliser **jetons =** à spécifier jusqu'à 26 jetons, sous réserve que cela n’entraîne pas une tentative déclarer une variable supérieure à la lettre « z » ou « Z ».

L’exemple suivant énumère les noms de variables d’environnement dans l’environnement actuel. Pour analyser la sortie d’une commande en plaçant *définir* entre les parenthèses, tapez :
```
for /f "usebackq delims==" %i in ('set') do @echo %i 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
