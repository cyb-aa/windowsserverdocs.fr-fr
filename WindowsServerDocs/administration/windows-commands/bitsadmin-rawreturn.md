---
title: bitsadmin rawreturn
description: La rubrique commandes Windows pour **Bitsadmin rawReturn**, qui retourne des données adaptées à l’analyse.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8cd84b6082b1fda8061ee7b324dd66991d3b206
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849882"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Retourne les données appropriées pour l’analyse. En règle générale, vous utilisez cette commande conjointement avec les commutateurs **/Create** et **/Get*** pour recevoir uniquement la valeur. Vous devez spécifier ce commutateur avant les autres commutateurs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /rawreturn
```

## <a name="remarks"></a>Notes

- Supprime les caractères de saut de ligne et la mise en forme de la sortie.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère les données brutes pour l’état de la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /rawreturn /getstate myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)