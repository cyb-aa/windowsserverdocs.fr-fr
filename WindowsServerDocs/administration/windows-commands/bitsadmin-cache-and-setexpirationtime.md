---
title: bitsadmin cache and setexpirationtime
description: Rubrique de référence pour le cache Bitsadmin et la commande setexpirationtime, qui définit le délai d’expiration du cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84679eadc750637fb720a458d9663219dc1492a4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718306"
---
> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>bitsadmin cache and setexpirationtime

Définit le délai d’expiration du cache.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /cache /setexpirationtime secs
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| secs | Nombre de secondes avant l’expiration du cache. |

## <a name="examples"></a>Exemples

Pour définir l’expiration du cache dans 60 secondes :

```
bitsadmin /cache / setexpirationtime 60
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande de cache Bitsadmin](bitsadmin-cache.md)
