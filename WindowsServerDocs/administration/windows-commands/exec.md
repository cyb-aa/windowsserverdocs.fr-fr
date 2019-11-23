---
title: exécutable
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 514503e4920e16ba6778185af32f925541805223
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377433"
---
# <a name="exec"></a>exécutable



exécute un fichier sur l’ordinateur local. Le fichier peut être un script **cmd** .

## <a name="syntax"></a>Syntaxe

```
exec <ScriptFile.cmd>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ScriptFile. cmd >|Spécifie le fichier de script à exécuter.|

## <a name="remarks"></a>Notes

-   Cette commande permet de dupliquer ou de restaurer des données dans le cadre d’une séquence de sauvegarde ou de restauration.
-   Si le script échoue, une erreur est retournée et DiskShadow s’arrête.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)