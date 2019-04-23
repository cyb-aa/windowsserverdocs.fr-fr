---
title: bitsadmin info
description: Rubrique de commandes de Windows pour **affiche des informations résumées concernant le travail spécifié.** -bitsadmin info
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ee96c69e311600a53f04b1b883983718adf0f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851520"
---
# <a name="bitsadmin-info"></a>bitsadmin info



Affiche des informations résumées concernant le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Info <Job> [/verbose]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

Utilisez le /verbose paramètre pour fournir des informations détaillées sur la tâche.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère des informations sur la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /Info myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)