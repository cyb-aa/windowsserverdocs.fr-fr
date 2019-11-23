---
title: supprimer les ombres
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c965af8b045c5ab3a110542d148b255f382a95c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378628"
---
# <a name="delete-shadows"></a>supprimer les ombres



supprime les clichés instantanés.

## <a name="syntax"></a>Syntaxe

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

## <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                             Description                                                                              |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        tous        |                                                                      Supprime tous les clichés instantanés.                                                                      |
| volume \<volume >  |                                                            Supprime tous les clichés instantanés du volume donné.                                                            |
| > du volume de \<le plus ancien  |                                                         Supprime le cliché instantané le plus ancien du volume donné.                                                          |
|   définir \<SetID >    | Supprime les clichés instantanés dans le jeu de clichés instantanés de l’ID donné. Vous pouvez spécifier un alias à l’aide du symbole **%** si l’alias existe dans l’environnement actuel. |
|  ID \<ShadowID >   |              Supprime un cliché instantané de l’ID donné. Vous pouvez spécifier un alias à l’aide du symbole **%** si l’alias existe dans l’environnement actuel.               |
| > du lecteur {\<exposé |                                                                            <MountPoint>}                                                                             |

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)