---
title: ftype
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c29ca0aa027d11fa8f981134e5367021227d3096
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881680"
---
# <a name="ftype"></a>ftype



Affiche ou modifie les types de fichiers qui sont utilisés dans les associations d’extension de nom de fichier. Si utilisé sans un opérateur d’assignation (**=**), **ftype** affiche la chaîne de commande d’ouverture actuelle pour le type de fichier spécifié. Si utilisée sans paramètres, **ftype** affiche les types de fichiers qui ont des commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<FileType>|Spécifie le type de fichier pour afficher ou modifier.|
|\<OpenCommandString>|Spécifie la chaîne de commande d’ouverture à utiliser lors de l’ouverture des fichiers du type de fichier spécifié.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant décrit comment **ftype** substitue des variables au sein d’une chaîne de commande d’ouverture :

|Variable|Valeur de remplacement|
|--------|-----------------|
|%0 ou %1|Est remplacé par le nom de fichier qui est lancé via l’association.|
|%*|Obtient tous les paramètres.|
|%2, %3, ...|Obtient le premier paramètre (%2), le deuxième paramètre (%3) et ainsi de suite.|
|%~\<N>|Obtient tous les paramètres restants, en commençant par le *N*ième paramètre ni où *N* peut être n’importe quel nombre de 2 à 9.|

## <a name="BKMK_examples"></a>Exemples

Pour afficher les types de fichiers en cours qui ont des commandes, tapez :
```
ftype
```
Pour afficher la chaîne de commande d’ouverture actuelle pour le *txtfile* type de fichier, type :
```
ftype txtfile
```
Cette commande produit une sortie similaire à ce qui suit :
```
txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
```
Pour supprimer la chaîne de commande d’ouverture pour un type de fichier appelé *exemple*, type :
```
ftype example=
```
Pour associer l’extension de nom de fichier .pl avec le type de fichier PerlScript et activer le type de fichier PerlScript exécuter PERL. EXE, tapez les commandes suivantes :
```
assoc .pl=PerlScript 
ftype PerlScript=perl.exe %1 %*
```
Pour éliminer le besoin d’entrer l’extension de nom de fichier .pl lors de l’appel d’un script Perl, tapez :
```
set PATHEXT=.pl;%PATHEXT%
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)