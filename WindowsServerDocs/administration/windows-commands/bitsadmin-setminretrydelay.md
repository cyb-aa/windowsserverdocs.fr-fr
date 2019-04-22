---
title: bitsadmin setminretrydelay
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 640492cf690a934e3e3b8d0ecf8ca7a0d6a7dc2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813080"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Définit la durée minimale, en secondes, que le service BITS attend après la survenue d’une erreur temporaire avant de tenter de transférer le fichier.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetMinRetryDelay <Job> <RetryDelay>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|RetryDelay|Un nombre représenté en secondes.|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit le délai minimum de nouvelle tentative pour le travail nommé *myDownloadJob* à 35 secondes.
```
C:\>bitsadmin /SetMinRetryDelay myDownloadJob 35
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)