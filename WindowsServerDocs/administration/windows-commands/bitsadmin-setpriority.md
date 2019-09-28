---
title: bitsadmin setpriority
description: Rubrique relative aux commandes Windows pour **Bitsadmin SetPriority** -définit la priorité du travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60564350928f917ca1861684e042304d5d380426
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380437"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority



Définit la priorité du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetPriority <Job> <Priority>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Priority|L’une des valeurs suivantes :</br>-PREMIER PLAN</br>-ÉLEVÉ</br>-NORMAL</br>-FAIBLE|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant définit la priorité de la tâche nommée *myDownloadJob* sur normal.
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)