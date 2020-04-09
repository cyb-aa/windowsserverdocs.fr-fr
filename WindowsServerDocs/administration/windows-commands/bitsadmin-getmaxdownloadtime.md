---
title: Bitsadmin getmaxdownloadtime
description: La rubrique commandes Windows pour **Bitsadmin getmaxdownloadtime**, qui récupère le délai de téléchargement en secondes.
ms.prod: windows-servemr
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b6e4a45da76d5ba39edae151454ad7f28a74085
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850632"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>Bitsadmin getmaxdownloadtime

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère le délai de téléchargement en secondes.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getmaxdownloadtime <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant obtient le temps de téléchargement maximal pour la tâche nommée *myDownloadJob* en secondes.

```
C:\>bitsadmin /getmaxdownloadtime myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
