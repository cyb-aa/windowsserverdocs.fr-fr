---
title: bitsadmin suspend
description: Rubrique de référence pour la commande Bitsadmin suspend, qui interrompt le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8117cf9f4286994847e53dca8065da6821d47c5d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720452"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompt le travail spécifié. Si vous avez interrompu votre travail par erreur, vous pouvez utiliser le commutateur de [reprise Bitsadmin](bitsadmin-resume.md) pour redémarrer le travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /suspend <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ---------- |
| travail | Nom complet ou GUID du travail. |

## <a name="example"></a> Exemple

Pour interrompre la tâche nommée *myDownloadJob*:


```
bitsadmin /suspend myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin Resume](bitsadmin-resume.md)

- [commande Bitsadmin](bitsadmin.md)
