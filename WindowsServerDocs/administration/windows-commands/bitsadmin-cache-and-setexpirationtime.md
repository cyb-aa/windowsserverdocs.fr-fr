---
title: cache Bitsadmin et setexpirationtime
description: La rubrique commandes Windows pour le **cache Bitsadmin et setexpirationtime** -définit le délai d’expiration du cache.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 386c6659e4410b41669ade39d8af97829d81a1cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381919"
---
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>cache Bitsadmin et setexpirationtime
Définit le délai d’expiration du cache.
## <a name="syntax"></a>Syntaxe
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|secs|Nombre de secondes avant l’expiration du cache.|
## <a name="BKMK_examples"></a>Illustre
L’exemple suivant fait expirer le cache en 60 secondes.
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
