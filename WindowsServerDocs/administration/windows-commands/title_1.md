---
title: title
description: Référence pour title, qui crée un titre pour la fenêtre d’invite de commandes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a94fe033bfd43d825c5beb7c915937bc4419b18f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721336"
---
# <a name="title"></a>title

Crée un titre pour la fenêtre d’invite de commandes.



## <a name="syntax"></a>Syntaxe

```
title [<String>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<> de chaîne|Spécifie le titre de la fenêtre d’invite de commandes.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   Pour créer un titre de fenêtre pour les programmes batch, incluez la commande **title** au début d’un programme batch.
-   Une fois que vous avez défini un titre de fenêtre, vous pouvez le réinitialiser uniquement à l’aide de la commande **titre** .

## <a name="examples"></a>Exemples

Dans l’exemple de script suivant, le titre de la fenêtre d’invite de commandes est remplacé par la mise à jour des fichiers, tandis que le fichier de commandes exécute la commande de **copie** . Une fois la commande exécutée, le `Files Updated` texte est affiché et le titre de la fenêtre d’invite de commandes est rétabli en invite de commandes.
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)