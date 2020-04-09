---
title: supprimer les ombres
description: La rubrique commandes Windows pour supprimer les ombres, qui supprime les clichés instantanés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd109f7ddc0365d03737eddba31a1a4b7f34915b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846562"
---
# <a name="delete-shadows"></a>supprimer les ombres

supprime les clichés instantanés.

## <a name="syntax"></a>Syntaxe

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ---- | ---- |
| tous | Supprime tous les clichés instantanés. |
| volume \<volume > | Supprime tous les clichés instantanés du volume donné. |
| > du volume de \<le plus ancien | Supprime le cliché instantané le plus ancien du volume donné. |
| définir \<SetID > | Supprime les clichés instantanés dans le jeu de clichés instantanés de l’ID donné. Vous pouvez spécifier un alias à l’aide du symbole **%** si l’alias existe dans l’environnement actuel. |
| ID \<ShadowID > | Supprime un cliché instantané de l’ID donné. Vous pouvez spécifier un alias à l’aide du symbole **%** si l’alias existe dans l’environnement actuel. |
| > du lecteur {\<exposé | <MountPoint>} |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)