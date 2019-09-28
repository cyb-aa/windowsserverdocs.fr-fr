---
title: title
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 42094e0f1231fee5ac9ef0ec9184ba685c8846b1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385793"
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
|@no__t 0String >|Spécifie le titre de la fenêtre d’invite de commandes.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour créer un titre de fenêtre pour les programmes batch, incluez la commande **title** au début d’un programme batch.
-   Une fois que vous avez défini un titre de fenêtre, vous pouvez le réinitialiser uniquement à l’aide de la commande **titre** .

## <a name="BKMK_examples"></a>Illustre

Dans l’exemple de script suivant, le titre de la fenêtre d’invite de commandes est remplacé par « mise à jour des fichiers », tandis que le fichier de commandes exécute la commande de **copie** . Une fois la commande exécutée, le texte `Files Updated` s’affiche et le titre de la fenêtre d’invite de commandes est rétabli en « invite de commandes ».
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)