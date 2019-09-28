---
title: créer
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2245efb6c3bce8aecf8edf730694804ffbdc3d80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378771"
---
# <a name="create"></a>créer



démarre le processus de création de clichés instantanés à l’aide du contexte et des paramètres d’option actuels. Nécessite au moins un volume dans le jeu de clichés instantanés.

## <a name="syntax"></a>Syntaxe

```
create
```

## <a name="remarks"></a>Notes

-   Vous devez ajouter au moins un volume avec la commande **Add volume** avant de pouvoir utiliser la commande **Create** .
-   Vous pouvez utiliser la commande **Démarrer la sauvegarde** pour spécifier une sauvegarde complète plutôt qu’une sauvegarde de copie.
-   Après avoir exécuté la commande **Create** , vous pouvez utiliser la commande **Exec** pour exécuter un script de duplication pour la sauvegarde à partir du cliché instantané.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)