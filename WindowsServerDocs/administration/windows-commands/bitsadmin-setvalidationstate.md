---
title: Bitsadmin setvalidationstate
description: La rubrique commandes Windows pour **Bitsadmin setvalidationstate** -définit l’état de validation du contenu du fichier donné au sein du travail.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37d7fa3a8a91abf1e7b6ac5a51b6cebd78984a91
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380400"
---
# <a name="bitsadmin-setvalidationstate"></a>Bitsadmin setvalidationstate



Définit l’état de validation du contenu du fichier donné au sein du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

## <a name="parameters"></a>Paramètres

| Paramètre  |          Description           |
|------------|--------------------------------|
|    Tâche     | Nom complet ou GUID du travail |
| Index de fichiers |         Commence à 0          |
|    True    |             False              |

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant définit l’état de validation du contenu du fichier 2 sur TRUE pour le travail nommé *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)