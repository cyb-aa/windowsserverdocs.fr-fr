---
title: créer
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 70a8f53d9cb90fc36a76b11de2f93da71874617a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881270"
---
# <a name="create"></a>créer



démarre le processus de création de copie de clichés instantanés, en utilisant les paramètres de contexte et l’option actuelles. Nécessite au moins un volume dans le jeu de copie de clichés instantanés.

## <a name="syntax"></a>Syntaxe

```
create
```

## <a name="remarks"></a>Notes

-   Vous devez ajouter au moins un volume avec le **ajouter un volume** commande avant de pouvoir utiliser le **créer** commande.
-   Vous pouvez utiliser la **commencer sauvegarde** commande pour spécifier une sauvegarde complète, plutôt que d’une sauvegarde de copie.
-   Après avoir exécuté le **créer** commande, vous pouvez utiliser la **exec** commande à exécuter un script de duplication pour la sauvegarde à partir du cliché.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)