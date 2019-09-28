---
title: setlocal
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4e4b6d3-3f1a-4851-a782-25ee2470e16e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 997c996854f488bb1776f135e3288e3b094e683c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384088"
---
# <a name="setlocal"></a>setlocal



Démarre la localisation des variables d’environnement dans un fichier de commandes. La localisation se poursuit jusqu’à ce qu’une commande **endlocal** correspondante soit rencontrée ou que la fin du fichier de commandes soit atteinte.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

## <a name="arguments"></a>Arguments

|Argument|Description|
|--------|-----------|
|enableextensions|Active les extensions de commande jusqu’à ce que la commande **endlocal** correspondante soit rencontrée, quel que soit le paramètre avant l’exécution de la commande **setlocal** .|
|disableextensions|Désactive les extensions de commande jusqu’à ce que la commande **endlocal** correspondante soit rencontrée, quel que soit le paramètre avant l’exécution de la commande **setlocal** .|
|enabledelayedexpansion|Active l’expansion variable d’environnement retardée jusqu’à ce que la commande **endlocal** correspondante soit rencontrée, quel que soit le paramètre avant l’exécution de la commande **setlocal** .|
|disabledelayedexpansion|Désactive l’expansion de la variable d’environnement différée jusqu’à ce que la commande **endlocal** correspondante soit rencontrée, quel que soit le paramètre avant l’exécution de la commande **setlocal** .|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Utilisation de **setlocal**

    Quand vous utilisez **setlocal** en dehors d’un script ou d’un fichier de commandes, il n’a aucun effet.
-   Modification des variables d’environnement

    Utilisez **setlocal** pour modifier les variables d’environnement lorsque vous exécutez un fichier de commandes. Les modifications de l’environnement effectuées après l’exécution de **setlocal** sont locales dans le fichier de commandes. Le programme cmd. exe restaure les paramètres précédents lorsqu’il rencontre une commande **endlocal** ou atteint la fin du fichier de commandes.
-   Imbrication de commandes

    Vous pouvez avoir plusieurs commandes **setlocal** ou **endlocal** dans un programme de traitement par lots (c’est-à-dire, des commandes imbriquées).
-   Test des extensions de commande dans les fichiers de commandes

    La commande **setlocal** définit la variable ERRORLEVEL. Si vous transmettez {**enableextensions** | **DISABLEEXTENSIONS**} ou {**ENABLEDELAYEDEXPANSION** | **disabledelayedexpansion**}, la variable ERRORLEVEL est définie sur **0** (zéro). Dans le cas contraire, elle a la valeur **1**. Vous pouvez utiliser ces informations dans des scripts batch pour déterminer si les extensions sont disponibles, comme illustré dans l’exemple suivant :  
    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```  
    Comme **cmd** ne définit pas la variable ERRORLEVEL lorsque les extensions de commande sont désactivées, la commande **verify** initialise la variable ERRORLEVEL à une valeur différente de zéro quand vous l’utilisez avec un argument non valide. En outre, si vous utilisez la commande **setlocal** avec les arguments {**ENABLEEXTENSIONS** | **disableextensions**} ou {**ENABLEDELAYEDEXPANSION** | **disabledelayedexpansion**} et qu’elle ne définit pas la variable ERRORLEVEL sur **1**, les extensions de commande ne sont pas disponibles.

## <a name="BKMK_examples"></a>Illustre

Vous pouvez localiser les variables d’environnement dans un fichier de commandes, comme indiqué dans l’exemple de script suivant :
```
rem *******Begin Comment**************
rem This program starts the superapp batch program on the network,
rem directs the output to a file, and displays the file
rem in Notepad.
rem *******End Comment**************
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)