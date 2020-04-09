---
title: assoc
description: La rubrique commandes Windows pour Assoc, qui affiche ou modifie les associations d’extension de nom de fichier.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 442ba244a7325425df29a1ebdcdb8bc107095ebe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851292"
---
# <a name="assoc"></a>assoc

Affiche ou modifie les associations d’extension de nom de fichier. S’il est utilisé sans paramètres, **Assoc** affiche une liste de toutes les associations d’extensions de nom de fichier actuelles.

> [!NOTE]
> Cette commande est uniquement prise en charge dans CMD. EXE et n’est pas disponible à partir de PowerShell.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
assoc [<.ext>[=[<FileType>]]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<.ext>` | Spécifie l’extension de nom de fichier. |
| `<FileType>` | Spécifie le type de fichier à associer à l’extension de nom de fichier spécifiée. |
| `/?` | Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes

- Pour supprimer l’Association de types de fichiers pour une extension de nom de fichier, ajoutez un espace après le signe égal en appuyant sur la barre d’espace.

- Pour afficher les types de fichiers actuels qui ont des chaînes de commande ouvertes définies, utilisez la commande **ftype** .

- Pour rediriger la sortie d' **Assoc** vers un fichier texte, utilisez l’opérateur de redirection **>** .

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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
