---
title: more
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 93ba6c696c509ea20ffe8f680d4416d24d202b89
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437320"
---
# <a name="more"></a>more



Affiche un écran de sortie à la fois.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
<Command> | more [/c] [/p] [/s] [/t<N>] [+<N>]
more [[/c] [/p] [/s] [/t<N>] [+<N>]] < [<Drive>:][<Path>]<FileName>
more [/c] [/p] [/s] [/t<N>] [+<N>] [<Files>]
```

## <a name="parameters"></a>Paramètres

|           Paramètre            |                               Description                               |
|--------------------------------|-------------------------------------------------------------------------|
|           \<Command>           |      Spécifie une commande pour laquelle vous souhaitez afficher la sortie.      |
|               /c               |               Efface l’écran avant d’afficher une page.               |
|               /p               |                      Développe des caractères de saut de page.                      |
|               /s               |          Affiche plusieurs lignes vides sous la forme d’une ligne vierge.          |
|             /t\<N>             |         Affiche les onglets en tant que le nombre d’espaces spécifié par *N*.         |
|             +\<N>              |     Affiche le premier fichier commençant à la ligne spécifiée par *N*.     |
| [\<Drive>:] [<Path>]<FileName> |          Spécifie l’emplacement et le nom d’un fichier à afficher.          |
|            \<Files>            | Spécifie une liste de fichiers à afficher. Séparez les noms de fichiers par un espace. |
|               /?               |                  Affiche l'aide à l'invite de commandes.                   |

## <a name="remarks"></a>Notes

-   Les sous-commandes suivantes sont acceptées à le **plus** invite (`-- More --`).  
    |Touche|Action|
    |---|------|
    |BARRE D’ESPACE|Affiche la page suivante.|
    |Entrée|Affiche la ligne suivante.|
    |f|Affiche le fichier suivant.|
    |q|Quitte le **plus** commande.|
    |=|Affiche le numéro de ligne.|
    |p \<N>|Affiche la prochaine *N* lignes.|
    |s \<N>|Ignore la prochaine *N* lignes.|
    |?|Affiche les commandes qui sont disponibles sur le **plus** invite.|
-Lorsque vous utilisez le caractère de redirection ( **<** ), vous devez spécifier un nom de fichier comme source. Lorsque vous utilisez le canal (**|*), vous pouvez utiliser ces commandes en tant que **dir**, **tri**, et **type**.
-   Le **plus** commande, avec des paramètres différents, est disponible à partir de la Console de récupération.

## <a name="BKMK_examples"></a>Exemples

Pour afficher le premier écran d’informations d’un fichier nommé clients.nou, tapez une des commandes suivantes :
```
more < clients.new
type clients.new | more
```
Le **plus** commande affiche le premier écran d’informations de clients.nou, puis affiche l’invite suivante :
```
-- More --
```
Vous pouvez ensuite appuyer sur la barre d’espace à l’écran suivant affiche des informations.

Pour effacer l’écran et supprimer toutes les lignes vierges supplémentaires avant d’afficher le fichier clients.nou, tapez une des commandes suivantes :
```
more /c /s < clients.new
type clients.new | more /c /s
```
Le **plus** commande affiche le premier écran d’informations de clients.nou, puis affiche l’invite suivante :
```
-- More --
```

### <a name="using-more-subcommands"></a>À l’aide de plusieurs sous-commandes

Les exemples suivants peuvent être utilisés à la **plus** invite (`-- More --`).
- Pour afficher le fichier une ligne à la fois, appuyez sur entrée à la **plus** invite.
- Pour afficher l’écran suivant, appuyez sur la barre d’espace à la **plus** invite.
- Pour afficher le prochain fichier indiqué sur la ligne de commande, tapez **f** à la **plus** invite.
- Pour afficher les commandes disponibles, tapez **?** à la **plus** invite.
- Pour quitter **plus**, type **q** à la **plus** invite.
- Pour afficher le numéro de ligne actuel, tapez **=** à la **plus** invite. Le numéro de ligne est ajouté à la **plus** invite comme suit :  
  ```
  -- More [Line: 24] --
  ```  
- Pour afficher un nombre spécifique de lignes, tapez **p** à la **plus** invite. **Plus** vous invite à entrer le nombre de lignes à afficher comme suit :  
  ```
  -- More -- Lines:
  ```  
  Tapez le nombre de lignes à afficher, puis appuyez sur ENTRÉE. **Plus** affiche le nombre de lignes spécifié.
- Pour ignorer un nombre spécifique de lignes, tapez **s** à la **plus** invite. **Plus** vous invite à entrer le nombre de lignes à ignorer comme suit :  
  ```
  -- More -- Lines:
  ```  
  Tapez le nombre de lignes à ignorer, puis appuyez sur ENTRÉE. **Plus** ignore le nombre spécifié de lignes et affiche l’écran suivant de plus d’informations.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)