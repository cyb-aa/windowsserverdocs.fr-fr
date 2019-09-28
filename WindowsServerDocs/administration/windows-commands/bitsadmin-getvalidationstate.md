---
title: Bitsadmin getvalidationstate
description: 'La rubrique commandes Windows pour **Bitsadmin getvalidationstate** -signale l’état de validation du contenu du fichier donné au sein du travail. '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca4269a596010258edd0479f5a7e9844bc9c98df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381258"
---
# <a name="bitsadmin-getvalidationstate"></a>Bitsadmin getvalidationstate



Signale l’état de validation du contenu du fichier donné au sein du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetValidationState <Job> <file index> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Index de fichiers|Commence à 0|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant obtient l’état de validation du contenu du fichier 2 au sein du travail nommé *myJob*.
```
C:\>bitsadmin /GetValidationState myJob 1
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)