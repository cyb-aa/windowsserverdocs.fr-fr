---
title: setexpirationtime et bitsadmin cache
description: Rubrique de commandes de Windows pour **bitsadmin cache et setexpirationtime** -définit le délai d’expiration de cache.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e896df0a88c0cfc4eec07aba4807f184e7abe32
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66008937"
---
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>setexpirationtime et bitsadmin cache
Définit le délai d’expiration de cache.
## <a name="syntax"></a>Syntaxe
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|secondes|Le nombre de secondes avant l’expiration du cache.|
## <a name="BKMK_examples"></a>Exemples
L’exemple suivant l’expiration du cache en 60 secondes.
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
