---
title: title
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1d1ea70849c3beb4503edfdaa5116384c14a2fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848500"
---
# <a name="title"></a>title



Crée un titre pour la fenêtre d’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
title [<String>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<chaîne >|Spécifie le titre de la fenêtre d’invite de commandes.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour créer le titre de la fenêtre pour les programmes de traitement par lots, incluez le **titre** commande au début d’un programme de traitement par lots.
-   Un titre de fenêtre est désactivée, vous pouvez le réinitialiser uniquement en utilisant le **titre** commande.

## <a name="BKMK_examples"></a>Exemples

Dans l’exemple de script, le titre de la fenêtre d’invite de commandes est remplacé par « Fichiers de la mise à jour » pendant que le fichier de commandes s’exécute le **copie** commande. Une fois que la commande est exécutée, le texte `Files Updated` s’affiche, et le titre de la fenêtre d’invite de commandes est modifié au « Invite de commandes ».
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)