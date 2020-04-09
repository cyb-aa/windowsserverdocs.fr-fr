---
title: title
description: La rubrique commandes Windows pour titre, qui crée un titre pour la fenêtre d’invite de commandes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aae18e97daaef226443c5f18c6c401a4e2b82b59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832802"
---
# <a name="title"></a>title

Crée un titre pour la fenêtre d’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
title [<String>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Chaîne \<>|Spécifie le titre de la fenêtre d’invite de commandes.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Pour créer un titre de fenêtre pour les programmes batch, incluez la commande **title** au début d’un programme batch.
-   Une fois que vous avez défini un titre de fenêtre, vous pouvez le réinitialiser uniquement à l’aide de la commande **titre** .

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Dans l’exemple de script suivant, le titre de la fenêtre d’invite de commandes est remplacé par la mise à jour des fichiers, tandis que le fichier de commandes exécute la commande de **copie** . Une fois la commande exécutée, le texte `Files Updated` s’affiche et le titre de la fenêtre d’invite de commandes est rétabli en invite de commandes.
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)