---
title: supprimer les ombres
description: Rubrique de référence pour supprimer les ombres, qui supprime les clichés instantanés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dd367d76ad1699321af9caf47a0ddc351088a05
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720798"
---
# <a name="delete-shadows"></a>supprimer les ombres

Supprime les clichés instantanés.

## <a name="syntax"></a>Syntaxe

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ---- | ---- |
| all | Supprime tous les clichés instantanés. |
| \<volume de volume> | Supprime tous les clichés instantanés du volume donné. |
| Volume \<le plus ancien> | Supprime le cliché instantané le plus ancien du volume donné. |
| définir \<le> SetID | Supprime les clichés instantanés dans le jeu de clichés instantanés de l’ID donné. Vous pouvez spécifier un alias à l’aide **%** du symbole si l’alias existe dans l’environnement actuel. |
| ID \<ShadowID> | Supprime un cliché instantané de l’ID donné. Vous pouvez spécifier un alias à l’aide **%** du symbole si l’alias existe dans l’environnement actuel. |
| {\<Drive> exposé | <MountPoint>} |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)