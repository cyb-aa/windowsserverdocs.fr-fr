---
title: Bitsadmin setmaxdownloadtime
description: Rubrique relative aux commandes Windows pour Bitsadmin setmaxdownloadtime, qui définit le délai d’attente de téléchargement en secondes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd44011cd14d575a9c3798ede45641fac4c3dc75
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849372"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>Bitsadmin setmaxdownloadtime

Définit le délai d’attente de téléchargement en secondes.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetMaxDownloadTime <Job> <Timeout>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Délai d'expiration|Délai d’expiration en secondes|

## <a name="remarks"></a>Notes

-   N/A

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant définit le délai d’attente pour la tâche nommée *myDownloadJob* sur 10 secondes.
```
C:\>bitsadmin /SetMaxDownloadTime myDownloadJob 10
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)