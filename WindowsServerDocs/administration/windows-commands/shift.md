---
title: déplacement
description: Rubrique de référence pour Shift, qui modifie la position des paramètres de lot dans un fichier de commandes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 617d7f712ccae13c522bc65db147c12c526b4efb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721829"
---
# <a name="shift"></a>déplacement

Modifie la position des paramètres de lot dans un fichier de commandes.



## <a name="syntax"></a>Syntaxe

```
shift [/n <N>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/n \<n>|Spécifie de commencer le décalage au *N*ième argument, où *N* est une valeur comprise entre 0 et 8. Requiert des extensions de commande, qui sont activées par défaut.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

- La commande **Shift** modifie les valeurs des paramètres de lot **%0** à **%9** en copiant chaque paramètre dans le précédent : la valeur **%1** est copiée dans **%0**, la valeur **%2** est copiée dans **%1**, et ainsi de suite. Cela est utile pour écrire un fichier de commandes qui effectue la même opération sur un nombre quelconque de paramètres.
- Si les extensions de commande sont activées, la commande **Shift** prend en charge l’option de ligne de commande **/n** . L’option **/n** spécifie de commencer le décalage au nième argument, où **n** est une valeur comprise entre 0 et 8. Par exemple, **Shift/2** déplace **%3** vers **%2**, **%4** vers **%3**, et ainsi de suite, et laisse **%0** et **%1** non affectés. Les extensions de commande sont activées par défaut.
- Vous pouvez utiliser la commande **Shift** pour créer un fichier de commandes qui peut accepter plus de 10 paramètres batch. Si vous spécifiez plus de 10 paramètres sur la ligne de commande, ceux qui apparaissent après le dixième (**%9**) sont décalés un à la fois dans **%9**.
- La commande **Shift** n’a aucun effet sur ** % **le paramètre * batch.
- Il n’y a pas de commande de **décalage vers** l’arrière. Après avoir implémenté la commande **Shift** , vous ne pouvez pas récupérer le paramètre batch (**%0**) qui existait avant le décalage.

## <a name="examples"></a>Exemples

Les lignes suivantes d’un exemple de fichier de commandes nommé Mycopy. bat montrent comment utiliser **Shift** avec un nombre quelconque de paramètres de lot. Dans cet exemple, Mycopy. bat copie une liste de fichiers dans un répertoire spécifique. Les paramètres de lot sont représentés par les arguments de nom de fichier et de répertoire.
```
@echo off 
rem MYCOPY.BAT copies any number of files
rem to a directory.
rem The command uses the following syntax:
rem mycopy dir file1 file2 ... 
set todir=%1
:getfile
shift
if %1== goto end
copy %1 %todir%
goto getfile
:end
set todir=
echo All done
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)