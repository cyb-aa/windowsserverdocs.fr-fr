---
title: ftype
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f982f68f25a4decbc9c572b533fa1ecc5e893a8c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842702"
---
# <a name="ftype"></a>ftype



Affiche ou modifie les types de fichiers utilisés dans les associations d’extension de nom de fichier. S’il est utilisé sans opérateur d’assignation ( **=** ), **ftype** affiche la chaîne de commande actuellement ouverte pour le type de fichier spécifié. En cas d’utilisation sans paramètre, **ftype** affiche les types de fichiers pour lesquels des chaînes de commande ouvertes sont définies.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<FileType >|Spécifie le type de fichier à afficher ou à modifier.|
|\<OpenCommandString >|Spécifie la chaîne de commande ouverte à utiliser lors de l’ouverture de fichiers du type de fichier spécifié.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant décrit comment **ftype** remplace les variables dans une chaîne de commande ouverte :

|Variable|Valeur de remplacement|
|--------|-----------------|
|%0 ou %1|Est remplacé par le nom de fichier qui est lancé via l’Association.|
|%*|Obtient tous les paramètres.|
|%2, %3,...|Obtient le premier paramètre (%2), le deuxième paramètre (%3), et ainsi de suite.|
|%~\<N >|Obtient tous les paramètres restants à partir du *n*ième paramètre, où *N* peut être un nombre compris entre 2 et 9.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour afficher les types de fichiers actuels pour lesquels des chaînes de commande ouvertes sont définies, tapez :
```
ftype
```
Pour afficher la chaîne de commande actuellement ouverte pour le type de fichier *txtfile* , tapez :
```
ftype txtfile
```
Cette commande produit une sortie similaire à ce qui suit :
```
txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
```
Pour supprimer la chaîne de commande ouverte pour un type de fichier appelé *exemple*, tapez :
```
ftype example=
```
Pour associer l’extension de nom de fichier. pl au type de fichier PerlScript et activer le type de fichier PerlScript pour exécuter PERL. EXE, tapez les commandes suivantes :
```
assoc .pl=PerlScript 
ftype PerlScript=perl.exe %1 %*
```
Pour éviter d’avoir à taper l’extension de nom de fichier. pl lors de l’appel d’un script Perl, tapez :
```
set PATHEXT=.pl;%PATHEXT%
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)