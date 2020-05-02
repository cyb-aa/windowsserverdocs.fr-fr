---
title: bitsadmin getmaxdownloadtime
description: Rubrique de référence pour la commande Bitsadmin getmaxdownloadtime, qui récupère le délai d’attente de téléchargement en secondes.
ms.prod: windows-servemr
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cdce64f6-7125-489d-be3c-4af1dfc8c46a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c63acee7629267ed10df11fb8cf4eeb0c911e118
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717866"
---
# <a name="bitsadmin-getmaxdownloadtime"></a>bitsadmin getmaxdownloadtime

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère le délai de téléchargement en secondes.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getmaxdownloadtime <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour obtenir le temps de téléchargement maximal pour le travail nommé *myDownloadJob* en secondes :

```
bitsadmin /getmaxdownloadtime myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
