---
title: rem
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85c8a69bf21a386cd36e45bbca6dacd35aef2509
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847000"
---
# <a name="rem"></a>rem



Enregistre un commentaire dans un fichier de commandes ou de la configuration. SYS. Si aucun commentaire n’est spécifié, **rem** ajoute l’espacement vertical.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
rem [<Comment>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Commentaire >|Spécifie une chaîne de caractères à inclure sous forme de commentaire.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Le **rem** commande n’affiche pas les commentaires sur l’écran. Vous devez utiliser le **écho** commande dans votre configuration ou un lot. Sys pour afficher des commentaires sur l’écran.
-   Vous ne pouvez pas utiliser un caractère de redirection (**<** ou **>**) ou canal (**|**) dans un commentaire sur le fichier batch.
-   Bien que vous puissiez utiliser **rem** sans commentaire pour ajouter l’espacement vertical à un fichier de commandes, vous pouvez également utiliser des lignes vides. Lignes vides sont ignorés lors du traitée d’un programme de traitement par lots.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant montre un fichier de commandes qui utilise des remarques pour les commentaires et pour l’espacement vertical :
```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause 
format b: /v chkdsk b: 
```
Pour inclure un commentaire explicatif avant la **invite** commande dans votre fichier CONFIG. SYS, ajoutez les lignes suivantes à la configuration. SYS :
```
rem Set prompt to indicate current directory
prompt $p$g
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)