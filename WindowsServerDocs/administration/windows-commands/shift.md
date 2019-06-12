---
title: shift
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e72b4be1b265d682d489cf372cdfe5ef54bb444d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441246"
---
# <a name="shift"></a>shift



Modifie l’emplacement des paramètres dans un fichier de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
shift [/n <N>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/n \<N>|Spécifie pour démarrer le décalage au *N*argument th, où *N* est toute valeur comprise entre 0 et 8. Requiert des extensions de commande, qui sont activées par défaut.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

- Le **MAJ** commande modifie les valeurs des paramètres de lot **%0** via **%9** en copiant chaque paramètre dans le précédent, la valeur de **%1** est copié vers **%0**, la valeur de **%2** est copié vers **%1**, et ainsi de suite. Cela est utile pour l’écriture d’un fichier de commandes qui effectue la même opération sur n’importe quel nombre de paramètres.
- Si les extensions de commande sont activées, le **MAJ** commande prend en charge la **/n** option de ligne de commande. Le **/n** option indique de démarrer le décalage au N-ième argument, où **N** est toute valeur comprise entre 0 et 8. Par exemple, **MAJ /2** décale **%3** à **%2**, **%4** à **%3**, et ainsi de suite et laissez **%0** et **%1** pas affectées. Les extensions de commande sont activées par défaut.
- Vous pouvez utiliser la **MAJ** commande pour créer un fichier de commandes qui peut accepter plus de 10 paramètres de traitement par lots. Si vous spécifiez plus de 10 paramètres sur la ligne de commande, ceux qui apparaissent après le dixième ( **%9**) sera décalée à la fois dans **%9**.
- Le **MAJ** commande n’a aucun effet le **% \\** * paramètre du lot.
- Il est descendante ne **MAJ** commande. Une fois que vous implémentez le **MAJ** commande, vous ne pouvez pas récupérer le paramètre de lot ( **%0**) qui existait avant le déplacement.

## <a name="BKMK_examples"></a>Exemples

Les lignes suivantes à partir d’un exemple de fichier batch appelée appelé MaCopie.bat montrent comment utiliser **MAJ** avec n’importe quel nombre de paramètres de traitement par lots. Dans cet exemple, appelé MaCopie.bat copie une liste de fichiers dans un répertoire spécifique. Les paramètres de traitement par lots sont représentées par les arguments de nom de répertoire et fichier.
```
@echo off 
rem MYCOPY.BAT copies any number of files
rem to a directory.
rem The command uses the following syntax:
rem mycopy dir file1 file2 ... 
set todir=%1
:getfile
shift
if "%1"=="" goto end
copy %1 %todir%
goto getfile
:end
set todir=
echo All done
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)