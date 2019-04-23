---
title: bitsadmin geterror
description: Rubrique de commandes de Windows pour **bitsadmin geterror** -récupère des informations d’erreur pour le travail spécifié détaillées.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10a3373c0c8f290ff1f5f26ef38531fbc7745890
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889970"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror



Récupère les informations d’erreur détaillées pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetError <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère les informations d’erreur pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetError myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)