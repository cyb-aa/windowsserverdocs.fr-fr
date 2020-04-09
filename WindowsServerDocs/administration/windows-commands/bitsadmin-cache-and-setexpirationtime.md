---
title: cache Bitsadmin et setexpirationtime
description: Rubrique relative aux commandes Windows pour le **cache Bitsadmin et setexpirationtime**, qui définit le délai d’expiration du cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf283a0a8b94fd55c591609e3dcd1d127a2be81a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850882"
---
>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>cache Bitsadmin et setexpirationtime

Définit le délai d’expiration du cache.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /cache /setexpirationtime secs
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| secs | Nombre de secondes avant l’expiration du cache. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant fait expirer le cache en 60 secondes.

```
C:\>bitsadmin /cache / setexpirationtime 60
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
