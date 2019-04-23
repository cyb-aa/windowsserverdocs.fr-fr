---
title: Bitsadmin getvalidationstate
description: 'Rubrique de commandes de Windows pour **bitsadmin getvalidationstate** -signale l’état de validation du contenu du fichier donné au sein du travail. '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8abff3fc9fddb9cff1758739fdc540a9c945efe2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879160"
---
# <a name="bitsadmin-getvalidationstate"></a>Bitsadmin getvalidationstate



Signale l’état de validation du contenu du fichier donné au sein du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetValidationState <Job> <file index> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Fichier d’index|Démarre à 0|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant obtient l’état de validation du contenu du fichier 2 au sein du travail nommé *myJob*.
```
C:\>bitsadmin /GetValidationState myJob 1
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)