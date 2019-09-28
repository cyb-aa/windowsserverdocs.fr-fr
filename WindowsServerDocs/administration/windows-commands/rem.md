---
title: rem
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2da0b6e42858582c1485659f3bf8f59e8e2ed97e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384572"
---
# <a name="rem"></a>rem



Enregistre les commentaires (remarques) dans un fichier de commandes ou dans une configuration. Table. Si aucun commentaire n’est spécifié, **REM** ajoute l’espacement vertical.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
rem [<Comment>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0Comment >|Spécifie une chaîne de caractères à inclure comme commentaire.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   La commande **REM** n’affiche pas de commentaires à l’écran. Vous devez utiliser la commande **echo on** dans votre lot ou votre configuration. Fichier SYS pour afficher des commentaires à l’écran.
-   Vous ne pouvez pas utiliser un caractère de redirection ( **<** ou **>** ) ou un canal ( **|** ) dans un commentaire de fichier de commandes.
-   Bien que vous puissiez utiliser **REM** sans commentaire pour ajouter un espacement vertical à un fichier de commandes, vous pouvez également utiliser des lignes vides. Les lignes vides sont ignorées lors du traitement d’un programme de traitement par lots.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant montre un fichier de commandes qui utilise des remarques pour les commentaires et l’espacement vertical :
```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause 
format b: /v chkdsk b: 
```
Pour inclure un commentaire explicatif avant la commande **prompt** dans votre fichier config. Fichier SYS, ajoutez les lignes suivantes à la configuration. TABLE
```
rem Set prompt to indicate current directory
prompt $p$g
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)