---
title: bitsadmin suspend
description: 'Rubrique relative aux commandes Windows pour **Bitsadmin suspend** : interrompt le travail spécifié.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a3a484df2b50cdc8893512020b835f913793d2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380372"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Interrompt le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Suspend <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Pour redémarrer le travail, utilisez le commutateur de [reprise Bitsadmin](bitsadmin-resume.md) .

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant interrompt la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /Suspend myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
