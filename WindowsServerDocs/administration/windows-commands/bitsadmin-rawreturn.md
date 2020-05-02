---
title: bitsadmin rawreturn
description: Rubrique de référence pour la commande Bitsadmin rawReturn, qui retourne des données adaptées à l’analyse.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af465bb9f51ab6f43980c43bf2be1f5158429a82
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717083"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Retourne les données appropriées pour l’analyse. En règle générale, vous utilisez cette commande conjointement avec les commutateurs **/Create** et **/Get*** pour recevoir uniquement la valeur. Vous devez spécifier ce commutateur avant les autres commutateurs.

> [!NOTE]
> Cette commande supprime les caractères de saut de ligne et la mise en forme de la sortie.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /rawreturn
```

## <a name="examples"></a>Exemples

Pour récupérer les données brutes pour l’état de la tâche nommée *myDownloadJob*:

```
bitsadmin /rawreturn /getstate myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
