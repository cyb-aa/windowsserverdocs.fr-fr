---
title: if
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e56624a5c82caefdf7cc51c7a84f67882756e274
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724859"
---
# <a name="if"></a>if



Effectue un traitement conditionnel dans des programmes batch.



## <a name="syntax"></a>Syntaxe

```
if [not] ERRORLEVEL <Number> <Command> [else <Expression>]
if [not] <String1>==<String2> <Command> [else <Expression>]
if [not] exist <FileName> <Command> [else <Expression>]
```
Si les extensions de commande sont activées, utilisez la syntaxe suivante :
```
if [/i] <String1> <CompareOp> <String2> <Command> [else <Expression>]
if cmdextversion <Number> <Command> [else <Expression>]
if defined <Variable> <Command> [else <Expression>]
```

### <a name="parameters"></a>Paramètres

|        Paramètre        |                                                                                                                                                                                                                Description                                                                                                                                                                                                                 |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           not           |                                                                                                                                                                              Spécifie que la commande doit être exécutée uniquement si la condition est false.                                                                                                                                                                              |
|  numéro \<ERRORLEVEL>   |                                                                                                                                                      Spécifie une condition true uniquement si le programme précédent exécuté par cmd. exe a retourné un code de sortie égal ou supérieur au *nombre*.                                                                                                                                                       |
|       \<> de commande        |                                                                                                                                                                            Spécifie la commande qui doit être exécutée si la condition précédente est remplie.                                                                                                                                                                             |
|  \<Chaîne1>= =<String2>  |                                                                                                             Spécifie une condition true uniquement si *Chaîne1* et *Chaîne2* sont identiques. Ces valeurs peuvent être des chaînes littérales ou des variables de lot (par exemple, %1). Vous n’avez pas besoin de placer des chaînes littérales entre guillemets.                                                                                                              |
|    nom \<de fichier existant>    |                                                                                                                                                                                       Spécifie une condition true si le nom de fichier spécifié existe.                                                                                                                                                                                        |
|      \<CompareOp>       |                                                                               Spécifie un opérateur de comparaison à trois lettres. La liste suivante représente les valeurs valides pour *CompareOp*:</br>**EQU** Égal à</br>**NEQ** Différent de</br>**LSS** Inférieur à</br>**Leq** Inférieur ou égal à</br>**GTR** Supérieur à</br>**GEQ** Supérieur ou égal à                                                                                |
|           /i            |                                                            Force les comparaisons de chaînes à ignorer la casse.  Vous pouvez utiliser **/i** sur la forme <em>Chaîne1</em>**==**<em>Chaîne2</em> de **If**. Ces comparaisons sont génériques, en ce sens que si les deux *string1* et *Chaîne2* sont constitués uniquement de chiffres numériques, les chaînes sont converties en nombres et une comparaison numérique est effectuée.                                                            |
| valeur \<de CMDEXTVERSION> | Spécifie une condition true uniquement si le numéro de version interne associé à la fonctionnalité d’extensions de commande de cmd. exe est supérieur ou égal au nombre spécifié. La première version est 1. Elle augmente par incréments d’une lorsque des améliorations significatives sont ajoutées aux extensions de commande. La condition **cmdextversion** n’est jamais vraie lorsque les extensions de commande sont désactivées (par défaut, les extensions de commande sont activées). |
|   Variable \<définie>   |                                                                                                                                                                                            Spécifie une condition true si la *variable* est définie.                                                                                                                                                                                            |
|      \<Expression>      |                                                                                                                                                                   Spécifie une commande de ligne de commande et tous les paramètres à passer à la commande dans une clause **else** .                                                                                                                                                                   |
|           /?            |                                                                                                                                                                                                    Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                    |

## <a name="remarks"></a>Notes 

-   Si la condition spécifiée dans une clause **If** a la valeur true, la commande qui suit la condition est exécutée. Si la condition est false, la commande dans la clause **If** est ignorée et la commande exécute toute commande spécifiée dans la clause **else** .
-   Quand un programme s’arrête, il retourne un code de sortie. Pour utiliser des codes de sortie comme conditions, utilisez **ERRORLEVEL**.
-   Si vous utilisez **défini**, les trois variables suivantes sont ajoutées à l’environnement : **% ERRORLEVEL%**, **% cmdcmdline%** et **% cmdextversion%**.  
    -   **% ERRORLEVEL%** s’étend dans une représentation sous forme de chaîne de la valeur actuelle de la variable d’environnement ERRORLEVEL. Cela suppose qu’il n’existe pas de variable d’environnement avec le nom ERRORLEVEL. Si tel est le cas, vous obtiendrez la valeur ERRORLEVEL à la place.
    -   **% cmdcmdline%** s’étend sur la ligne de commande d’origine qui a été passée à cmd. exe avant tout traitement par cmd. exe. Cela suppose qu’il n’existe pas de variable d’environnement avec le nom CMDCMDLINE. Si tel est le cas, vous obtiendrez la valeur CMDCMDLINE à la place.
    -   **% cmdextversion%** s’étend dans la représentation sous forme de chaîne de la valeur actuelle de **cmdextversion**. Cela suppose qu’il n’existe pas de variable d’environnement avec le nom CMDEXTVERSION. Si tel est le cas, vous obtiendrez la valeur CMDEXTVERSION à la place.
-   Vous devez utiliser la clause **else** sur la même ligne que la commande après le **If**.

## <a name="examples"></a>Exemples

Pour afficher le message Impossible de trouver le fichier de données si le fichier Product. dat est introuvable, tapez :
```
if not exist product.dat echo Cannot find data file 
```
Pour formater un disque dans le lecteur A et afficher un message d’erreur si une erreur se produit pendant le processus de mise en forme, tapez les lignes suivantes dans un fichier de commandes :
```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```
Pour supprimer le fichier Product. dat du répertoire actif ou afficher un message si Product. dat est introuvable, tapez les lignes suivantes dans un fichier de commandes :
```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> Ces lignes peuvent être combinées en une seule ligne, comme suit :
> ```
> IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
> ```
> Pour répercuter la valeur de la variable d’environnement ERRORLEVEL après l’exécution d’un fichier de commandes, tapez les lignes suivantes dans le fichier de commandes :
> ```
> goto answer%errorlevel%
> :answer1
> echo The program returned error level 1
> goto end
> :answer0
> echo The program returned error level 0
> goto end
> :end
> echo Done! 
> ```
> Pour accéder à l’étiquette OK si la valeur de la variable d’environnement ERRORLEVEL est inférieure ou égale à 1, tapez :
> ```
> if %errorlevel% LEQ 1 goto okay
> ```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[If](if.md)

[Goto](goto.md)
