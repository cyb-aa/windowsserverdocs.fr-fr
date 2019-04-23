---
title: Bitsadmin setmaxdownloadtime
description: Rubrique de commandes de Windows pour **bitsadmin setmaxdownloadtime** -définit le délai d’attente de téléchargement en secondes.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f13b44429bec2718af1a648f273fead18d4e9e08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830990"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>Bitsadmin setmaxdownloadtime



Définit le délai d’attente de téléchargement en secondes.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Délai d’expiration dépassé|Le délai d’expiration en secondes|

## <a name="remarks"></a>Notes

-   N/A

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit le délai d’attente pour le travail nommé *myDownloadJob* à 10 secondes.
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)