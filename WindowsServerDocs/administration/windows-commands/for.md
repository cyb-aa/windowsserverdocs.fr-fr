---
title: pour
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: db0bf54e35e4226cb020b040d5fc36ddd88dc02b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377124"
---
# <a name="for"></a>pour



Exécute une commande spécifiée pour chaque fichier dans un ensemble de fichiers.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|{%% \|%} \<Variable >|Obligatoire. Représente un paramètre remplaçable. Utilisez un signe de pourcentage unique ( **%** ) pour exécuter la commande for à l’invite **de** commandes. Utilisez des signes de double pourcentage ( **%%** ) pour exécuter la commande for dans un fichier **de** commandes. Les variables respectent la casse et doivent être représentées par une valeur alphabétique telle que **% A**, **% B**ou **% C**.|
|(\<Set >)|Obligatoire. Spécifie un ou plusieurs fichiers, répertoires ou chaînes de texte, ou une plage de valeurs sur laquelle exécuter la commande. Les parenthèses sont obligatoires.|
|@no__t 0Command >|Obligatoire. Spécifie la commande que vous souhaitez exécuter sur chaque fichier, répertoire ou chaîne de texte, ou sur la plage de valeurs incluses dans l' *ensemble*.|
|@no__t 0CommandLineOptions >|Spécifie les options de ligne de commande que vous souhaitez utiliser avec la commande spécifiée.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Utilisation **de pour**

  Vous pouvez utiliser la commande **for** dans un fichier de commandes ou directement à partir de l’invite de commandes.
- Utilisation des paramètres batch

  Les attributs suivants s’appliquent à la commande **for** :  
  - La commande **for** remplace la variable **%** <em>ou la</em> variable **%%** <em>par chaque</em> chaîne de texte dans le jeu spécifié jusqu’à ce que la commande spécifiée traite tous les fichiers.
  - Les noms de variables respectent la casse, global et le nombre de 52 peut être actif à la fois.
  - Pour éviter toute confusion avec les paramètres de lot **% 0** à **% 9**, vous pouvez utiliser n’importe quel caractère pour une *variable* , à l’exception des chiffres de 0 à 9. Pour les fichiers de commandes simples, un seul caractère tel que **%% f** fonctionnera.
  - Vous pouvez utiliser plusieurs valeurs pour les *variables* dans des fichiers de commandes complexes afin de distinguer différentes variables remplaçables.
- Spécification d’un groupe de fichiers

  Le paramètre *Set* peut représenter un groupe de fichiers unique ou plusieurs groupes de fichiers. Vous pouvez utiliser des caractères génériques **&#42;** (et **?** ) pour spécifier un jeu de fichiers. Les ensembles de fichiers valides sont les suivants :  
  ```
  (*.doc) 
  (*.doc *.txt *.me)
  (jan*.doc jan*.rpt feb*.doc feb*.rpt)
  (ar??1991.* ap??1991.*)
  ```  
  Lorsque vous utilisez la commande **for** , la première valeur de *Set* remplace **%** <em>variable</em> ou **%%** <em>variable</em>, puis la commande spécifiée traite cette valeur. Cela se poursuit jusqu’à ce que tous les fichiers (ou groupes de fichiers) qui correspondent à la valeur *définie* soient traités.
- Utilisation des mots clés **in** et **do**

  **Dans** et **ne** sont pas des paramètres, mais vous devez les utiliser avec **pour**. Si vous omettez l’un de ces mots clés, un message d’erreur s’affiche.
- Utilisation de formulaires supplémentaires de **pour**

  Si les extensions de commande sont activées (valeur par défaut), les formes supplémentaires suivantes de **pour** sont prises en charge :  
  - Répertoires uniquement

    Si *Set* contient des caractères génériques **&#42;** (ou **?** ), la *commande* spécifiée s’exécute pour chaque répertoire (au lieu d’un ensemble de fichiers dans un répertoire spécifié) correspondant à *Set*.

    La syntaxe est :  
    ```
    for /d {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>] 
    ```  
  - Récursive

    Parcourt l’arborescence de répertoires qui est enracinée dans le*chemin d’accès* *lecteur*: et exécute l’instruction **for** dans chaque répertoire de l’arborescence. Si aucun répertoire n’est spécifié après **/r**, le répertoire actif est utilisé comme répertoire racine. Si *Set* n’est qu’un seul point (.), il n’énumère que l’arborescence de répertoires.

    La syntaxe est :  
    ```
    for /r [[<Drive>:]<Path>] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    ```  
  - Itération d’une plage de valeurs

    Utilisez une variable itérative pour définir la valeur de départ (*Start*#), puis parcourez une plage définie de valeurs jusqu’à ce que la valeur dépasse la valeur de fin définie (*end*#). **/l** exécute l’instruction itérative en comparant *Start*# avec *end*#. Si *Start*# est inférieur à *end*#, la commande s’exécute. Lorsque la variable itérative dépasse la *fin*#, l’interface de commande quitte la boucle. Vous pouvez également utiliser un numéro d' *étape*négatif pour parcourir une plage de valeurs décroissantes. Par exemple, (1, 1, 5) génère la séquence 1 2 3 4 5 et (5,-1, 1) génère la séquence 5 4 3 2 1.

    La syntaxe est :  
    ```
    for /l {%%|%}<Variable> in (<Start#>,<Step#>,<End#>) do <Command> [<CommandLineOptions>]
    ```  
  - Itération et analyse de fichiers

    Utilisez l’analyse de fichiers pour traiter la sortie de commande, les chaînes et le contenu du fichier.  Utilisez des variables itératives pour définir le contenu ou les chaînes que vous souhaitez examiner et utilisez les différentes options *ParsingKeywords* pour modifier davantage l’analyse.  Utilisez l’option de jeton *ParsingKeywords* pour spécifier les jetons qui doivent être transmis en tant que variables itératives. Notez que lorsqu’elle est utilisée sans l’option de jeton, **/f** n’examine que le premier jeton.

    L’analyse des fichiers consiste à lire la sortie, la chaîne ou le contenu du fichier, puis à la décomposer en lignes de texte individuelles et à analyser chaque ligne en zéro ou plusieurs jetons. La boucle **for** est ensuite appelée avec la valeur de la variable itérative définie sur le jeton. Par défaut, **/f** transmet le premier jeton séparé vide à partir de chaque ligne de chaque fichier. Les lignes vides sont ignorées.

    Les syntaxes sont les suivantes :  
    ```
    for /f ["<ParsingKeywords>"] {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
    for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ("<LiteralString>") do <Command> [<CommandLineOptions>]
    for /f ["<ParsingKeywords>"] {%%|%}<Variable> in ('<Command>') do <Command> [<CommandLineOptions>]
    ```  
    L’argument *Set* spécifie un ou plusieurs noms de fichiers. Chaque fichier est ouvert, lu et traité avant de passer au fichier suivant dans *Set*. Pour remplacer le comportement d’analyse par défaut, spécifiez *ParsingKeywords*. Il s’agit d’une chaîne entre guillemets qui contient un ou plusieurs mots clés pour spécifier des options d’analyse différentes.

    Si vous utilisez l’option **usebackq** , utilisez l’une des syntaxes suivantes :  
    ```
    for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ("<Set>") do <Command> [<CommandLineOptions>]
    for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in ('<LiteralString>') do <Command> [<CommandLineOptions>]
    for /f ["usebackq <ParsingKeywords>"] {%%|%}<Variable> in (`<Command>`) do <Command> [<CommandLineOptions>]
    ```  
    Le tableau suivant répertorie les mots clés d’analyse que vous pouvez utiliser pour *ParsingKeywords*.  

    |      Mot-clé      |                                                                                                                                                                                                          Description                                                                                                                                                                                                          |
    |-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |     EOL = \<c >      |                                                                                                                                                                                   Spécifie un caractère de fin de ligne (un seul caractère).                                                                                                                                                                                    |
    |     Skip = \<N >     |                                                                                                                                                                              Spécifie le nombre de lignes à ignorer au début du fichier.                                                                                                                                                                              |
    |   delims = \<xxx >   |                                                                                                                                                                     Spécifie un jeu de délimiteurs. Cela remplace l’ensemble de délimiteurs par défaut de l’espace et de la tabulation.                                                                                                                                                                      |
    | Tokens = \<X, Y, M – N > | Spécifie les jetons de chaque ligne à passer à la boucle **for** pour chaque itération. Par conséquent, des noms de variables supplémentaires sont alloués. *M*–*N* spécifie une plage, entre le *m*et les *N*ième jetons. Si le dernier caractère de la chaîne **tokens =** est un astérisque ( **&#42;** ), une variable supplémentaire est allouée et reçoit le texte restant sur la ligne après le dernier jeton analysé. |
    |     usebackq      |                                                                                             Spécifie : pour exécuter une chaîne entre guillemets en tant que commande, utilisez une chaîne entre guillemets simples comme chaîne littérale ou, pour les noms de fichiers longs qui contiennent des espaces, autorisez les noms de fichiers dans *\<set @ no__t-2*, à chacun placé entre guillemets doubles.                                                                                              |


  - Substitution de variable

    Le tableau suivant répertorie la syntaxe facultative (pour toutes les variables **I**).  

    |Variable avec modificateur|Description|
    |----------------------|-----------|
    |% ~ I|Développe **% I** , qui supprime tous les guillemets ("") qui l’entourent.|
    |% ~ fI|Développe **% I** en un nom de chemin d’accès complet.|
    |% ~ dI|Développe **% I** sur une lettre de lecteur uniquement.|
    |% ~ pI|Développe **% I** vers un chemin d’accès uniquement.|
    |% ~ nI|Développe **% I** vers un nom de fichier uniquement.|
    |% ~ xI|Développe **% I** vers une extension de nom de fichier uniquement.|
    |% ~ sI|Développe le chemin d’accès qui contient uniquement des noms courts.|
    |% ~ aI|Développe **% I** jusqu’aux attributs de fichier du fichier.|
    |% ~ tI|Développe **% I** jusqu’à la date et l’heure du fichier.|
    |% ~ zI|Développe **% I** jusqu’à la taille du fichier.|
    |% ~ $PATH : I|Recherche dans les répertoires figurant dans la variable d’environnement PATH et développe **% I** jusqu’au nom complet du premier répertoire trouvé. Si le nom de la variable d’environnement n’est pas défini ou si le fichier est introuvable par la recherche, ce modificateur se développe en une chaîne vide.|

    Le tableau suivant répertorie les combinaisons de modificateurs que vous pouvez utiliser pour obtenir des résultats composés.  

    |Variable avec modificateurs combinés|Description|
    |--------------------------------|-----------|
    |% ~ PPP|Développe **% I** vers une lettre de lecteur et un chemin d’accès uniquement.|
    |% ~ nxI|Développe **% I** en un nom de fichier et une extension uniquement.|
    |% ~ fsI|Développe **% I** en un nom de chemin d’accès complet avec des noms courts uniquement.|
    |% ~ DP $ chemin : I|Recherche dans les répertoires qui sont répertoriés dans la variable d’environnement PATH pour **% I** et s’étend sur la lettre de lecteur et le chemin d’accès du premier trouvé.|
    |% ~ ftzaI|Développe **% I** vers une ligne de sortie comme **dir**.|

    Dans les exemples ci-dessus, vous pouvez remplacer **% I** et PATH par d’autres valeurs valides. Un nom **de** variable valide pour termine la syntaxe **%~** .

    En utilisant des noms de variable en majuscules tels que **% I**, vous pouvez rendre votre code plus lisible et éviter toute confusion avec les modificateurs, qui ne respectent pas la casse.
- Analyse d’une chaîne

  Vous pouvez utiliser la logique **d’analyse pour/f** sur une chaîne immédiate en encapsulant *\<LiteralString @ no__t-3* dans les guillemets doubles (*sans* « usebackq ») ou en guillemets simples (*avec* « usebackq »), par exemple (« myString ») ou («» MyString'). *\<LiteralString @ no__t-2* est traitée comme une seule ligne d’entrée à partir d’un fichier. Lors de l’analyse de *\<LiteralString @ no__t-2* par des guillemets doubles, les symboles de commande (comme **\\ \& \| \> \<** \^) sont traités comme des caractères ordinaires.
- Analyse de la sortie

  Vous pouvez utiliser la commande **for/f** pour analyser la sortie d’une commande en plaçant un guillemet *\<command @ no__t-3* entre parenthèses. Elle est traitée comme une ligne de commande, qui est transmise à un enfant cmd. exe. La sortie est capturée en mémoire et analysée comme s’il s’agissait d’un fichier.

## <a name="BKMK_examples"></a>Illustre

Pour utiliser **pour** dans un fichier de commandes, utilisez la syntaxe suivante :
```
for {%%|%}<Variable> in (<Set>) do <Command> [<CommandLineOptions>]
```
Pour afficher le contenu de tous les fichiers du répertoire actif dont l’extension est. doc ou. txt à l’aide de la variable remplaçable **% f**, tapez :
```
for %f in (*.doc *.txt) do type %f 
```
Dans l’exemple précédent, chaque fichier ayant l’extension. doc ou. txt dans le répertoire actif est substitué à la variable **% f** jusqu’à ce que le contenu de chaque fichier soit affiché. Pour utiliser cette commande dans un fichier de commandes, remplacez toutes les occurrences de **% f** par **%% f**. Dans le cas contraire, la variable est ignorée et un message d’erreur s’affiche.

Pour analyser un fichier en ignorant les lignes commentées, tapez :
```
for /f "eol=; tokens=2,3* delims=," %i in (myfile.txt) do @echo %i %j %k
```
Cette commande analyse chaque ligne dans MyFile. txt. Elle ignore les lignes qui commencent par un point-virgule et transmet le deuxième et le troisième jeton de chaque ligne **au corps du (les jetons** sont délimités par des virgules ou des espaces). Le corps de l’instruction **for** fait référence à **% i** pour obtenir le deuxième jeton, **% j** pour obtenir le troisième jeton et **% k** pour obtenir tous les jetons restants. Si les noms de fichier que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte (par exemple, « nom de fichier »). Pour utiliser des guillemets, vous devez utiliser **usebackq**. Dans le cas contraire, les guillemets sont interprétés comme définissant une chaîne littérale à analyser.

**% i** est déclaré explicitement dans l’instruction **for** . **% j** et **% k** sont implicitement déclarés à l’aide de **tokens =** . Vous pouvez utiliser **tokens =** pour spécifier jusqu’à 26 jetons, à condition qu’il n’entraîne pas de tentative de déclaration d’une variable supérieure à la lettre « z » ou « z ».

L’exemple suivant énumère les noms des variables d’environnement dans l’environnement actuel. Pour analyser la sortie d’une commande en plaçant un *jeu* entre les parenthèses, tapez :
```
for /f "usebackq delims==" %i in ('set') do @echo %i 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
