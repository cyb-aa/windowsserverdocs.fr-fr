---
title: assoc
description: Rubrique de commandes de Windows pour **assoc** -affiche ou modifie les associations d’extension de nom de fichier.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d893167081b66c81366b59613c52182a4ddba370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816640"
---
# <a name="assoc"></a>assoc



Affiche ou modifie les associations d’extension de nom de fichier. Si utilisée sans paramètres, **assoc** affiche une liste de toutes les associations d’extension de nom du fichier actuel.

> [!NOTE]
> Cette commande est uniquement pris en charge dans cmd.exe. EXE et n’est pas disponible à partir de PowerShell.
>

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
assoc [<.ext>[=[<FileType>]]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|<.ext>|Spécifie l’extension de nom de fichier.|
|\<FileType>|Spécifie le type de fichier à associer à l’extension de nom de fichier spécifié.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour supprimer l’association de type de fichier pour une extension de nom de fichier, ajoutez un espace blanc après le signe égal en appuyant sur la barre d’espace.
-   Pour afficher les types de fichier qui ont des commandes, utilisez le **ftype** commande.
-   Pour rediriger la sortie de **assoc** dans un fichier texte, utilisez le **>** opérateur de redirection.

## <a name="BKMK_examples"></a>Exemples

Pour afficher l’association de type de fichier actuel pour l’extension de nom de fichier .txt, tapez :
```
assoc .txt
```
Pour supprimer l’association de type de fichier pour l’extension de nom de fichier .bak, tapez :
```
assoc .bak= 
```

> [!NOTE]
> Veillez à ajouter un espace après le signe égal.

Pour afficher la sortie de **assoc** un écran à la fois, type :
```
assoc | more
```
Pour envoyer la sortie de **assoc** vers le fichier assoc.txt, tapez :
```
assoc>assoc.txt
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
