---
title: bitsadmin getbytestransferred
description: Rubrique de commandes de Windows pour **bitsadmin getbytestransferred** -récupère le nombre d’octets transférés pour le travail spécifié.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cce2c051af169385c43fdff4efdeff46d8422926
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814610"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred



Récupère le nombre d’octets transférés pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetBytesTransferred <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère le nombre d’octets transférés pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetBytesTransferred myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)