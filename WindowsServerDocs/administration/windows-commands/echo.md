---
title: echo
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb9fcd0f-5e73-4504-aa95-78204e1a79d3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 343d6327d262401b4be14e472a135062456890f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377628"
---
# <a name="echo"></a>echo



Affiche des messages ou active ou désactive la fonctionnalité d’écho des commandes. En cas d’utilisation sans paramètre, **echo** affiche le paramètre d’écho actuel.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
echo [<Message>]
echo [on | off]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[sur \| OFF]|Active ou désactive la fonctionnalité d’écho de la commande. L’écho de la commande est activé par défaut.|
|@no__t 0Message >|Spécifie le texte à afficher à l’écran.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   La commande **echo** *message* est particulièrement utile lorsque **echo** est désactivé. Pour afficher un message de plusieurs lignes sans afficher de commandes, vous pouvez inclure plusieurs commandes de *message* d’écho après la commande **echo off** dans votre programme de traitement par lots.
-   Quand **echo** est désactivé, l’invite de commandes n’apparaît pas dans la fenêtre d’invite de commandes. Pour afficher l’invite de commandes, tapez **echo on.**
-   S’il est utilisé dans un fichier de commandes, **echo on** et **echo off** n’affectent pas le paramètre à l’invite de commandes.
-   Pour empêcher l’écho d’une commande particulière dans un fichier de commandes, insérez un arobase (@) devant la commande. Pour empêcher l’écho de toutes les commandes dans un fichier de commandes, incluez la commande **echo off** au début du fichier.
-   Pour afficher un canal ( **|** ) ou un caractère de redirection ( **<** ou **>** ) quand vous utilisez **echo**, utilisez un signe d’insertion (^) juste avant le caractère de redirection ou de redirection (par exemple, **^|** , **0** , ou **2**). Pour afficher un signe insertion, tapez deux signes de suite ( **^^** ).

## <a name="examples"></a>Exemples

Pour afficher le paramètre d' **écho** actuel, tapez :

```
echo
```

Pour renvoyer une ligne vide à l’écran, tapez :

```
echo.
```

> [!NOTE]
> N’incluez pas d’espace avant le point. Dans le cas contraire, le point s’affiche à la place d’une ligne vide.

Pour empêcher l’écho des commandes à l’invite de commandes, tapez :

```
echo off 
```

> [!NOTE]
> Quand **echo** est désactivé, l’invite de commandes n’apparaît pas dans la fenêtre d’invite de commandes. Pour afficher à nouveau l’invite de commandes, tapez **echo on**.

Pour empêcher que toutes les commandes dans un fichier de commandes (y compris la commande **echo off** ) s’affichent à l’écran, sur la première ligne du type de fichier de commandes :

```
@echo off
```

Vous pouvez utiliser la commande **echo** dans le cadre d’une instruction **If** . Par exemple, pour rechercher dans le répertoire actif tous les fichiers avec l’extension de nom de fichier. rpt et pour renvoyer un message si un tel fichier est trouvé, tapez :

```
if exist *.rpt echo The report has arrived.
```

Le fichier de commandes suivant recherche les fichiers avec l’extension de nom de fichier. txt dans le répertoire actif et affiche un message indiquant les résultats de la recherche :

```
@echo off
if not exist *.txt (
echo This directory contains no text files.
) else (
   echo This directory contains the following text files:
   echo.
   dir /b *.txt
   )
```

Si aucun fichier. txt n’est trouvé lors de l’exécution du fichier de commandes, le message suivant s’affiche :

```
This directory contains no text files.
```

Si des fichiers. txt sont trouvés quand le fichier de commandes est exécuté, les affichages de sortie suivants (pour cet exemple, supposent que les fichiers fichier1. txt, fichier2. txt et fichier3. txt existent) :

```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
