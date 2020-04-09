---
title: bitsadmin suspend
description: Rubrique relative aux commandes Windows pour Bitsadmin suspend, qui interrompt le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0419f4cdf59d04539b8b4c6d47cec886197d412b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849052"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interrompt le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Suspend <Job>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Pour redémarrer le travail, utilisez le commutateur de [reprise Bitsadmin](bitsadmin-resume.md) .

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant interrompt la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /Suspend myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
