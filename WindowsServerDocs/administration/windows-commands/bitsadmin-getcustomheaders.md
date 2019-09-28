---
title: Bitsadmin getcustomheaders
description: La rubrique commandes Windows pour **Bitsadmin getcustomheaders** -récupère les en-têtes HTTP personnalisés du travail.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 039669fca42803ff22eb4e3d13dfdef5f0a06f93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381662"
---
# <a name="bitsadmin-getcustomheaders"></a>Bitsadmin getcustomheaders



Récupère les en-têtes HTTP personnalisés du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetCustomHeaders <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant obtient les en-têtes personnalisés pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetCustomHeaders myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)