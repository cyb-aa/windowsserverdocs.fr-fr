---
title: rem
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94428e6d5ec6fdb482a5d0d15bd1120e45ffea80
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836112"
---
# <a name="rem"></a>rem



Enregistre les commentaires (remarques) dans un fichier de commandes ou dans une configuration. Table. Si aucun commentaire n’est spécifié, **REM** ajoute l’espacement vertical.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
rem [<Comment>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<le commentaire >|Spécifie une chaîne de caractères à inclure comme commentaire.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   La commande **REM** n’affiche pas de commentaires à l’écran. Vous devez utiliser la commande **echo on** dans votre lot ou votre configuration. Fichier SYS pour afficher des commentaires à l’écran.
-   Vous ne pouvez pas utiliser un caractère de redirection ( **<** ou **>** ) ou un canal ( **|** ) dans un commentaire de fichier de commandes.
-   Bien que vous puissiez utiliser **REM** sans commentaire pour ajouter un espacement vertical à un fichier de commandes, vous pouvez également utiliser des lignes vides. Les lignes vides sont ignorées lors du traitement d’un programme de traitement par lots.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)