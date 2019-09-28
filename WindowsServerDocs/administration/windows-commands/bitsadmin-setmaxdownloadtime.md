---
title: Bitsadmin setmaxdownloadtime
description: La rubrique commandes Windows pour **Bitsadmin setmaxdownloadtime** -définit le délai d’attente de téléchargement en secondes.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 985453de5bd2f4a06b5635ae5b0a9794d30175b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380559"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>Bitsadmin setmaxdownloadtime



Définit le délai d’attente de téléchargement en secondes.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Délai d’expiration dépassé|Délai d’expiration en secondes|

## <a name="remarks"></a>Notes

-   N/A

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant définit le délai d’attente pour la tâche nommée *myDownloadJob* sur 10 secondes.
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)