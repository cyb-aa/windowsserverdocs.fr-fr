---
title: plus
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/26/2019
ms.openlocfilehash: 4c627e003e71cb2265c717669e082d48564dd483
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839422"
---
# <a name="more"></a>plus



Affiche un écran de sortie à la fois.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
<Command> | more [/c] [/p] [/s] [/t<N>] [+<N>]
more [[/c] [/p] [/s] [/t<N>] [+<N>]] < [<Drive>:][<Path>]<FileName>
more [/c] [/p] [/s] [/t<N>] [+<N>] [<Files>]
```

### <a name="parameters"></a>Paramètres

|           Paramètre            |                               Description                               |
|--------------------------------|-------------------------------------------------------------------------|
|           > de commande \<           |      Spécifie une commande pour laquelle vous souhaitez afficher la sortie.      |
|               /c               |               Efface l’écran avant d’afficher une page.               |
|               /p               |                      Développe des caractères de saut de formulaire.                      |
|               /s               |          Affiche plusieurs lignes vides sous la forme d’une seule ligne vide.          |
|             /t\<N >             |         Affiche les tabulations sous la forme du nombre d’espaces spécifié par *N*.         |
|             +\<N >              |     Affiche le premier fichier en commençant à la ligne spécifiée par *N*.     |
| [\<> de lecteur :] [\<Path >]\<nom de fichier > |          Spécifie l’emplacement et le nom d’un fichier à afficher.          |
|            Fichiers de \<>            | Spécifie une liste de fichiers à afficher. Séparez les noms de fichiers par un espace. |
|               /?               |                  Affiche l'aide à l'invite de commandes.                   |

## <a name="remarks"></a>Notes

-   Les sous-commandes suivantes sont acceptées à l’invite **plus** (`-- More --`). 

    | Clé | Action |
    | --- | ------ |
    | TOUCHE | Affiche la page suivante. |
    | ENTRÉE | Affiche la ligne suivante. |
    | f | Affiche le fichier suivant. |
    | q | Quitte la commande **More** . |
    | = | Affiche le numéro de ligne. |
    | p \<N > | Affiche les *N* lignes suivantes. |
    | \<N > |Kips les *N* lignes suivantes. |
    | ? | Affiche les commandes qui sont disponibles à l’invite **plus** .| 
    
-   Lorsque vous utilisez le caractère de redirection ( **<** ), vous devez spécifier un nom de fichier comme source. Lorsque vous utilisez le canal ( **\|** ), vous pouvez utiliser des commandes telles que **dir**, **sort**et **type**.
-   La commande **More** , avec des paramètres différents, est disponible à partir de la console de récupération.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour afficher le premier écran d’informations d’un fichier nommé clients. nouveau, tapez l’une des commandes suivantes :
```
more < clients.new
type clients.new | more
```
La commande **More** affiche le premier écran d’informations de clients. New, puis affiche l’invite suivante :
```
-- More --
```
Vous pouvez ensuite appuyer sur la barre d’espace pour afficher l’écran d’informations suivant.

Pour effacer l’écran et supprimer toutes les lignes vides avant d’afficher le fichier clients. nouveau, tapez l’une des commandes suivantes :
```
more /c /s < clients.new
type clients.new | more /c /s
```
La commande **More** affiche le premier écran d’informations de clients. New, puis affiche l’invite suivante :
```
-- More --
```

### <a name="using-more-subcommands"></a>Utilisation de plusieurs sous-commandes

Les exemples suivants peuvent être utilisés à l’invite **plus** (`-- More --`).
- Pour afficher le fichier une ligne à la fois, appuyez sur entrée à l’invite **plus** .
- Pour afficher l’écran suivant, appuyez sur la barre d’espace à l’invite **plus** .
- Pour afficher le fichier suivant indiqué sur la ligne de commande, tapez **f** à l’invite **plus** .
- Pour afficher les commandes disponibles, tapez **?** à l’invite **plus** .
- Pour quitter **,** tapez **q** à l’invite **plus** .
- Pour afficher le numéro de ligne en cours, tapez **=** à l’invite **plus** . Le numéro de ligne en cours est ajouté à l’invite **More** comme suit :  
  ```
  -- More [Line: 24] --
  ```  
- Pour afficher un nombre spécifique de lignes, tapez **p** à l’invite **plus** . **Plus** vous invite à entrer le nombre de lignes à afficher comme suit :  
  ```
  -- More -- Lines:
  ```  
  Tapez le nombre de lignes à afficher, puis appuyez sur entrée. **Plus** affiche le nombre de lignes spécifié.
- Pour ignorer un nombre spécifique de lignes, tapez **s** à l’invite **plus** . **Plus** vous invite à entrer le nombre de lignes à ignorer comme suit :  
  ```
  -- More -- Lines:
  ```  
  Tapez le nombre de lignes à ignorer, puis appuyez sur entrée. **More** ignore le nombre spécifié de lignes et affiche l’écran d’informations suivant.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
