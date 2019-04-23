---
title: bitsadmin getcompletiontime
description: Rubrique de commandes de Windows pour **bitsadmin getcompletiontime** -récupère le temps que la tâche a terminé le transfert de données.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3790a91c4b347b982c0f0a023d5977a8d6cd1f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857380"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime



Récupère le temps que la tâche a terminé le transfert de données.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetCompletionTime <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère l’heure à laquelle le travail nommé *myDownloadJob* a terminé le transfert de données.
```
C:\>bitsadmin /GetCompletionTime myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)