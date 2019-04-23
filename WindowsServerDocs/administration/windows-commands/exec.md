---
title: EXEC
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ecdfd05b8abefb35946b783daaa3220a6713a38d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882920"
---
# <a name="exec"></a>EXEC



exécute un fichier sur l’ordinateur local. Le fichier peut être un **cmd** script.

## <a name="syntax"></a>Syntaxe

```
exec <ScriptFile.cmd>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ScriptFile.cmd>|Spécifie le fichier de script à exécuter.|

## <a name="remarks"></a>Notes

-   Cette commande permet de dupliquer ou de restaurer des données dans le cadre d’une sauvegarde ou de séquence de restauration.
-   Si le script échoue, une erreur est retournée et DiskShadow se ferme.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)