---
title: bitsadmin suspend
description: Rubrique relative aux commandes Windows pour **Bitsadmin suspend**, qui interrompt le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42ed83d4dbf8c3d982c5c186b440cf17997903c9
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123155"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompt le travail spécifié. Si vous avez interrompu votre travail par erreur, vous pouvez utiliser le commutateur de [reprise Bitsadmin](bitsadmin-resume.md) pour redémarrer le travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /suspend <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ---------- |
| Tâche | Nom complet ou GUID du travail. |

## <a name="example"></a>Exemple

L’exemple suivant interrompt la tâche nommée *myDownloadJob*.


```
C:\>bitsadmin /suspend myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
