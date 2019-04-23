---
title: bitsadmin getminretrydelay
description: Rubrique de commandes de Windows pour **bitsadmin getminretrydelay** -récupère la longueur de la durée, en secondes, pendant laquelle le service attend après la survenue d’une erreur temporaire avant de tenter de transférer le fichier.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2a6df9faab8340994ad9219a863ad8e50186ccd1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832200"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay



Récupère la longueur de la durée, en secondes, pendant laquelle le service attend après la survenue d’une erreur temporaire avant de tenter de transférer le fichier.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetMinRetryDelay <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère le délai minimum de nouvelle tentative pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetMinRetryDelay myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)