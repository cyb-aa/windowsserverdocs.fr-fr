---
title: bitsadmin rawreturn
description: La rubrique commandes Windows pour **Bitsadmin rawReturn** -retourne des données adaptées à l’analyse.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 86d769de460538acda696194348980de5752d6d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380878"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Retourne les données appropriées pour l’analyse.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /RawReturn
```

## <a name="remarks"></a>Notes

Supprime les caractères de saut de ligne et la mise en forme de la sortie.

En général, vous utilisez cette commande conjointement avec les commutateurs **Create** et **obtenir\\** * pour recevoir uniquement la valeur. Vous devez spécifier ce commutateur avant les autres commutateurs.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère les données brutes pour l’état de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /RawReturn /GetState myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)