---
title: bitsadmin rawreturn
description: Rubrique de commandes de Windows pour **bitsadmin rawreturn** -retourne des données approprié pour l’analyse.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e12c8e621021d35ac618b4592515fe38c36be0e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434893"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Retourne des données approprié pour l’analyse.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>Notes

Caractères de saut de ligne de bandes et mise en forme à partir de la sortie.

En règle générale, vous utilisez cette commande conjointement avec la **créer** et **obtenir\\** * commutateurs pour recevoir uniquement la valeur. Vous devez spécifier ce commutateur avant les autres commutateurs.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère les données brutes pour l’état de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)