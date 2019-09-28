---
title: bitsadmin getminretrydelay
description: La rubrique commandes Windows pour **Bitsadmin getminretrydelay** -récupère la durée, en secondes, pendant laquelle le service attend après avoir rencontré une erreur temporaire avant de tenter de transférer le fichier.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a2bde6340034e48b97b4c86f48a3b2ef72560a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381549"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay



Récupère la durée, en secondes, pendant laquelle le service attend après avoir rencontré une erreur temporaire avant de tenter de transférer le fichier.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetMinRetryDelay <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère le délai minimal entre deux tentatives pour la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetMinRetryDelay myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)