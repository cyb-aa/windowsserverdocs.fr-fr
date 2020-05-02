---
title: assoc
description: Rubrique de référence pour la commande Assoc, qui affiche ou modifie les associations d’extension de nom de fichier.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f637e1f744ec412899320cfbb368633b222da8d3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718954"
---
# <a name="assoc"></a>assoc

Affiche ou modifie les associations d’extension de nom de fichier. S’il est utilisé sans paramètres, **Assoc** affiche une liste de toutes les associations d’extensions de nom de fichier actuelles.

> [!NOTE]
> Cette commande est uniquement prise en charge dans CMD. EXE et n’est pas disponible à partir de PowerShell.

## <a name="syntax"></a>Syntaxe

```
assoc [<.ext>[=[<filetype>]]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<.ext>` | Spécifie l’extension de nom de fichier. |
| `<filetype>` | Spécifie le type de fichier à associer à l’extension de nom de fichier spécifiée. |
| /? | Affiche l'aide à l'invite de commandes. |

### <a name="remarks"></a>Notes 

- Pour supprimer l’Association de types de fichiers pour une extension de nom de fichier, ajoutez un espace après le signe égal en appuyant sur la barre d’espace.

- Pour afficher les types de fichiers actuels qui ont des chaînes de commande ouvertes définies, utilisez la commande **ftype** .

- Pour rediriger la sortie d' **Assoc** vers un fichier texte, utilisez l' `>` opérateur de redirection.

## <a name="examples"></a>Exemples

Pour afficher l’Association de type de fichier actuelle pour l’extension de nom de fichier. txt, tapez :

```
assoc .txt
```

Pour supprimer l’Association de types de fichiers pour l’extension de nom de fichier. bak, tapez :

```
assoc .bak= 
```

> [!NOTE]
> Veillez à ajouter un espace après le signe égal.

Pour afficher le résultat de l' **Association** d’un écran à la fois, tapez :

```
assoc | more
```

Pour envoyer la sortie d' **Assoc** au fichier Assoc. txt, tapez :

```
assoc>assoc.txt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande FTYPE](ftype.md)
