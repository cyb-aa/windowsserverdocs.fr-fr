---
title: Bitsadmin getmaxdownloadtime
description: La rubrique commandes Windows pour **Bitsadmin getmaxdownloadtime** -récupère le délai de téléchargement en secondes.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39a19f86e97c1a525b5beb0c5f3b23dff349cb19
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381578"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>Bitsadmin getmaxdownloadtime

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Récupère le délai de téléchargement en secondes.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetMaxDownloadtime <Job> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

-   N\/A

## <a name="BKMK_examples"></a>Illustre
L’exemple suivant obtient le temps de téléchargement maximal pour la tâche nommée *myDownloadJob* en secondes.

```
C:\>bitsadmin /GetMaxDownloadtime myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)


