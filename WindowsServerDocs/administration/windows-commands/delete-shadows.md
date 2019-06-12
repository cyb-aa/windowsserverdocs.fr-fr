---
title: supprimer les ombres
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1a0945477bc4fce907b5ec4a697c7a2ec2f59557
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436109"
---
# <a name="delete-shadows"></a>supprimer les ombres



Supprime les clichés instantanés.

## <a name="syntax"></a>Syntaxe

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

## <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                             Description                                                                              |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        tous        |                                                                      Supprime tous les clichés instantanés.                                                                      |
| volume \<Volume>  |                                                            Supprime tous les clichés instantanés du volume donné.                                                            |
| plus ancien \<Volume >  |                                                         Supprime l’ancienne copie de clichés instantanés de volume donné.                                                          |
|   Définissez \<SetID >    | Supprime les clichés instantanés dans le jeu de copie de clichés instantanés de l’ID donné. Vous pouvez spécifier un alias à l’aide de la **%** si l’alias existe dans l’environnement actuel de symboles. |
|  ID \<ShadowID >   |              Supprime un cliché instantané de l’ID donné. Vous pouvez spécifier un alias à l’aide de la **%** si l’alias existe dans l’environnement actuel de symboles.               |
| exposé {\<lecteur > |                                                                            <MountPoint>}                                                                             |

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)