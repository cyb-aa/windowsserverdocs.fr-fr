---
title: setlocal
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 70e58e3c3a7c3de594c620f7530816b57727d4c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868860"
---
# <a name="setlocal"></a>setlocal



Démarre la localisation des variables d’environnement dans un fichier de commandes. La localisation se poursuit jusqu'à ce que la mise en correspondance un **endlocal** commande est rencontrée ou à la fin du fichier de commandes est atteinte.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

## <a name="arguments"></a>Arguments

|Argument|Description|
|--------|-----------|
|enableextensions|Active les extensions de commande jusqu'à ce que la mise en correspondance **endlocal** commande est rencontrée, indépendamment du paramètre avant le **setlocal** commande a été exécutée.|
|disableextensions|Désactive les extensions de commandes jusqu'à ce que la mise en correspondance **endlocal** commande est rencontrée, indépendamment du paramètre avant le **setlocal** commande a été exécutée.|
|enabledelayedexpansion|Permet à l’expansion de variables d’environnement retardées jusqu'à ce que la mise en correspondance **endlocal** commande est rencontrée, indépendamment du paramètre avant le **setlocal** commande a été exécutée.|
|disabledelayedexpansion|Désactive l’expansion de variables d’environnement retardées jusqu'à ce que la mise en correspondance **endlocal** commande est rencontrée, indépendamment du paramètre avant le **setlocal** commande a été exécutée.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   À l’aide de **setlocal**

    Lorsque vous utilisez **setlocal** en dehors d’un fichier de script ou lot, il n’a aucun effet.
-   Modification des variables d’environnement

    Utilisez **setlocal** pour modifier les variables d’environnement lorsque vous exécutez un fichier de commandes. Modifications d’environnement apportées après l’exécution de **setlocal** locaux pour le fichier de commandes. Le programme Cmd.exe restaure les paramètres précédents lorsqu’il rencontre un **endlocal** commande ou atteint la fin du fichier de commandes.
-   Commandes d’imbrication

    Vous pouvez avoir plusieurs **setlocal** ou **endlocal** commande dans un programme de traitement par lots (autrement dit, les commandes imbriquées).
-   Test des extensions de commande dans des fichiers batch

    Le **setlocal** commande définit la variable ERRORLEVEL. Si vous passez {**enableextensions** | **disableextensions**} ou {**enabledelayedexpansion**  |   **DISABLEDELAYEDEXPANSION**}, la variable ERRORLEVEL est définie sur **0** (zéro). Sinon, il est défini sur **1**. Vous pouvez utiliser ces informations dans les scripts de commandes pour déterminer si les extensions sont disponibles, comme indiqué dans l’exemple suivant :  
    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```  
    Étant donné que **cmd** ne définit pas la variable ERRORLEVEL lorsque les extensions de commande sont désactivées, le **vérifier** commande initialise la variable ERRORLEVEL une valeur différente de zéro lorsque vous l’utilisez avec non valide argument. En outre, si vous utilisez le **setlocal** commande avec les arguments {**enableextensions** | **disableextensions**} ou {**enabledelayedexpansion**   |  **disabledelayedexpansion**} et il n’affecte pas la variable ERRORLEVEL **1**, les extensions de commande ne sont pas disponibles.

## <a name="BKMK_examples"></a>Exemples

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

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)