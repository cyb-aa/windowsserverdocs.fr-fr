---
title: if
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2fafd14b0b620a8b5630c869e33c5dbc7cd902f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869230"
---
# <a name="if"></a>if



Effectue un traitement conditionnel dans les programmes de traitement par lots.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

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

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|not|Spécifie que la commande doit être exécutée uniquement si la condition est fausse.|
|ERRORLEVEL \<numéro >|Spécifie une condition true uniquement si le programme précédent exécuté par Cmd.exe a renvoyé un code de sortie égal à ou supérieur à *nombre*.|
|\<Command>|Spécifie la commande qui doit être exécutée si la condition précédente est remplie.|
|\<String1>==<String2>|Spécifie un si seule condition vraie *String1* et *String2* sont les mêmes. Ces valeurs peuvent être des chaînes littérales ou des variables de traitement par lots (par exemple, %1). Vous n’avez pas besoin encadrer des chaînes littérales entre guillemets.|
|existe \<FileName >|Spécifie une condition vraie si le nom de fichier spécifié existe.|
|\<CompareOp>|Spécifie un opérateur de comparaison de trois lettres. La liste suivante représente les valeurs valides pour *OpComparaison*:</br>**EQU** égal à</br>**NEQ** non égal à</br>**LSS** inférieur à</br>**LEQ** inférieur ou égal à</br>**GTR** supérieur à</br>**GEQ** supérieur ou égal à|
|/i|Force les comparaisons pour ignorer la casse de chaînes.  Vous pouvez utiliser **/i** sur le *String1***==*** String2* formulaire de **si**. Ces comparaisons sont génériques, que si les deux *String1* et *String2* sont constitués de chiffres uniquement, les chaînes sont converties en nombres et une comparaison numérique est effectuée.|
|plutôt sa \<numéro >|Spécifie un uniquement si de condition vraie le numéro de version interne associé aux extensions de commande fonctionnalité de Cmd.exe est égale à ou supérieur au nombre spécifié. La première version est 1. Elle augmente par incréments de 1 lorsque des améliorations significatives sont ajoutées aux extensions de commande. Le **plutôt sa** conditionnelle est commande jamais la valeur true lorsque les extensions sont désactivées (par défaut, la commande extensions sont activées).|
|défini \<Variable >|Spécifie une condition vraie si *Variable* est défini.|
|\<Expression>|Spécifie une ligne de commande et tous les paramètres à passer à la commande dans un **else** clause.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Si la condition spécifiée dans une **si** clause a la valeur true, la commande qui suit la condition est exécutée. Si la condition est false, la commande dans le **si** clause est ignorée et la commande exécute toute commande qui est spécifié dans le **else** clause.
-   Lorsqu’un programme s’arrête, il renvoie un code de sortie. Pour utiliser les codes de sortie en tant que conditions, utilisez **errorlevel**.
-   Si vous utilisez **défini**, les trois variables suivantes sont ajoutées à l’environnement : **%ERRORLEVEL%**, **%cmdcmdline%**, et **plutôt sa %**.  
    -   **%ERRORLEVEL%** se développe en une représentation sous forme de chaîne de la valeur actuelle de la variable d’environnement ERRORLEVEL. Cela suppose qu’il y n'est pas une variable d’environnement existante portant le nom ERRORLEVEL, s’il existe, vous obtenez cette valeur ERRORLEVEL à la place.
    -   **% de sa** développe la ligne de commande d’origine qui a été passée à Cmd.exe avant tout traitement Cmd.exe. Cela suppose qu’il y n'est pas une variable d’environnement existante portant le nom de sa, s’il existe, vous obtenez la valeur de sa à la place.
    -   **%cmdextversion%** se développe en une représentation de chaîne de la valeur actuelle de **plutôt sa**. Cela suppose qu’il y n'est pas une variable d’environnement existante portant le nom plutôt sa, s’il existe, vous obtiendrez plutôt sa valeur à la place.
-   Vous devez utiliser le **else** clause sur la même ligne que la commande après la **si**.

## <a name="BKMK_examples"></a>Exemples

Pour afficher le message « Impossible de trouver le fichier de données » si le fichier produit.dat est introuvable, tapez :
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
Pour supprimer le fichier produit.dat depuis le répertoire actif ou afficher un message si produit.dat est introuvable, tapez les lignes suivantes dans un fichier de commandes :
```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> Ces lignes peuvent être combinées en une seule ligne, comme suit :
```
IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
```
Pour renvoyer la valeur de la variable d’environnement ERRORLEVEL après l’exécution d’un fichier de commandes, tapez les lignes suivantes dans le fichier de commandes :
```
goto answer%errorlevel%
:answer1
echo Program had return code 1
:answer0
echo Program had return code 0
goto end
:end
echo Done! 
```
Pour accéder à l’étiquette « OK » si la valeur de la variable d’environnement ERRORLEVEL est inférieure ou égale à 1, type :
```
if %errorlevel% LEQ 1 goto okay
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[If](if.md)

[Goto](goto.md)