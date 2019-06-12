---
title: echo
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bfe6c936ee5606e286aab076bea08db04b8b6500
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811167"
---
# <a name="echo"></a>echo



Affiche des messages ou Active ou désactive l’écho des commandes. Si utilisée sans paramètres, **echo** affiche le paramètre actuel.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
echo [<Message>]
echo [on | off]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[sur \| off]|Active ou désactive l’écho des commandes. Affichage des commandes est activée par défaut.|
|\<Message>|Spécifie le texte à afficher sur l’écran.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Le **echo** *Message* commande est particulièrement utile lorsque **echo** est désactivée. Pour afficher un message de plusieurs lignes de long sans afficher toutes les commandes, vous pouvez inclure plusieurs **echo** *Message* commandes après le **echo désactivé** commande dans votre programme de traitement par lots.
-   Lorsque **echo** est désactivée, l’invite de commandes n’apparaît pas dans la fenêtre d’invite de commandes. Pour afficher l’invite de commandes, tapez **écho.**
-   Si utilisé dans un fichier de commandes, **écho** et **echo désactivé** n’affectent pas le paramètre à l’invite de commandes.
-   Pour éviter d’afficher une commande particulière dans un fichier de commandes, insérez un signe arobase (@) devant la commande. Pour éviter l’écho de toutes les commandes dans un fichier de commandes, vous devez inclure le **echo désactivé** commande au début du fichier.
-   Pour afficher une barre verticale ( **|** ) ou le caractère de redirection ( **<** ou **>** ) lorsque vous utilisez **echo**, utilisez un accent circonflexe (^) immédiatement avant le caractère de canal ou de la redirection (par exemple, **^|** , **^>** , ou **^<** ). Pour afficher un accent circonflexe, tapez deux signes successivement ( **^^** ).

## <a name="examples"></a>Exemples

Pour afficher les cours **echo** paramètre, tapez :

```
echo
```

Pour renvoyer une ligne vide dans l’écran, tapez :

```
echo.
```

> [!NOTE]
> N’incluez pas d’espace avant la période. Sinon, la période s’affichera au lieu d’une ligne vide.

Pour empêcher reproduit en écho les commandes à l’invite de commandes, tapez :

```
echo off 
```

> [!NOTE]
> Lorsque **echo** est désactivée, l’invite de commandes n’apparaît pas dans la fenêtre d’invite de commandes. Pour afficher à nouveau l’invite de commandes, tapez **echo sur**.

Pour empêcher toutes les commandes dans un fichier de commandes (y compris le **echo désactivé** commande) à partir de l’affichage à l’écran, sur la première ligne du type de fichier batch :

```
@echo off
```

Vous pouvez utiliser la **echo** commande en tant que partie d’un **si** instruction. Par exemple, pour rechercher le répertoire actif pour n’importe quel fichier avec l’extension de nom de fichier .rpt et renvoyer un message si un tel fichier est trouvé, tapez :

```
if exist *.rpt echo The report has arrived.
```

Le fichier de commandes suivant recherche dans le répertoire actuel pour les fichiers portant l’extension de nom de fichier .txt et affiche un message indiquant les résultats de la recherche :

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

Si vous ne trouve aucun fichier .txt lorsque le fichier de commandes est exécuté, le message suivant s’affiche :

```
This directory contains no text files.
```

Si les fichiers .txt sont trouvés, lorsque le fichier de commandes est exécuté la sortie suivante s’affiche (dans cet exemple, supposons fichiers File1.txt, File2.txt, File3.txt existe) :

```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
