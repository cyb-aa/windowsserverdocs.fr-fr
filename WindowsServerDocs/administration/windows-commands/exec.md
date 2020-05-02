---
title: exec
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f10e28a8da96bc7228af4561fb36824899f2d7a4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725738"
---
# <a name="exec"></a>exec



Exécute un fichier sur l’ordinateur local. Le fichier peut être un script **cmd** .

## <a name="syntax"></a>Syntaxe

```
exec <ScriptFile.cmd>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ScriptFile. cmd>|Spécifie le fichier de script à exécuter.|

## <a name="remarks"></a>Notes 

-   Cette commande permet de dupliquer ou de restaurer des données dans le cadre d’une séquence de sauvegarde ou de restauration.
-   Si le script échoue, une erreur est retournée et DiskShadow s’arrête.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)