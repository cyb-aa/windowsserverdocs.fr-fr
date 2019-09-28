---
title: bitsadmin setdescription
description: La rubrique commandes Windows pour **Bitsadmin SetDescription** -définit la description du travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d140ee9d575828a1a4d536073e468c9b4e56799f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380928"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription



Définit la description du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetDescription <Job> <Description>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Description|Texte utilisé pour décrire le travail.|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère la description de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /SetDescription myDownloadJob "Music Downloads"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)