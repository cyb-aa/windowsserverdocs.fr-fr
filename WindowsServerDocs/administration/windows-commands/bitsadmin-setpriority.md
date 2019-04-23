---
title: bitsadmin setpriority
description: Rubrique de commandes de Windows pour **bitsadmin setpriority** -définit la priorité de la tâche spécifiée.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 072f22ae8c928d427104062b8cbf0f8f42ac4416
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882210"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority



Définit la priorité de la tâche spécifiée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetPriority <Job> <Priority>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Priority|Une des valeurs suivantes :</br>-PREMIER PLAN</br>-ÉLEVÉ</br>-   NORMAL</br>-   LOW|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit la priorité du travail *myDownloadJob* à la normale.
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)