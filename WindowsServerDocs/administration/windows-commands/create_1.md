---
title: créer
description: Commande Windows pour Create, qui démarre le processus de création de clichés instantanés, à l’aide du contexte et des paramètres d’option actuels.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d29285517ca678a15828079c95663fc4d501eaf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846832"
---
# <a name="create"></a>créer

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