---
title: Bitsadmin getmaxdownloadtime
description: Rubrique de commandes de Windows pour **bitsadmin getmaxdownloadtime** -récupère le délai d’attente de téléchargement en secondes.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 868f7ac58a69c067681bf0597524fbaa5a25984f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842420"
---
#<a name="bitsadmin-getmaxdownloadtime"></a>Bitsadmin getmaxdownloadtime

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère le délai de téléchargement en secondes.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetMaxDownloadtime <Job> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

-   N\/A

## <a name="BKMK_examples"></a>Exemples
L’exemple suivant obtient le temps de téléchargement maximale pour le travail nommé *myDownloadJob* en secondes.

```
C:\>bitsadmin /GetMaxDownloadtime myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)


