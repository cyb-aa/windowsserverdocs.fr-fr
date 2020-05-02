---
title: create
description: Rubrique de référence pour Create, qui démarre le processus de création de clichés instantanés, à l’aide du contexte et des paramètres d’option actuels.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfddbebd5744d8cd222d67e46690ce8b5d2e0fde
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716834"
---
# <a name="create"></a>create

Démarre le processus de création de clichés instantanés à l’aide du contexte et des paramètres d’option actuels. Nécessite au moins un volume dans le jeu de clichés instantanés.

## <a name="syntax"></a>Syntaxe

```
create
```

## <a name="remarks"></a>Notes

-   Vous devez ajouter au moins un volume avec la commande **Add volume** avant de pouvoir utiliser la commande **Create** .
-   Vous pouvez utiliser la commande **Démarrer la sauvegarde** pour spécifier une sauvegarde complète plutôt qu’une sauvegarde de copie.
-   Après avoir exécuté la commande **Create** , vous pouvez utiliser la commande **Exec** pour exécuter un script de duplication pour la sauvegarde à partir du cliché instantané.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)