---
title: bitsadmin setdescription
description: Rubrique de commandes de Windows pour **bitsadmin setdescription** -définit la description de la tâche spécifiée.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8e3323c20eebc8ba633ccfd478daa0753e506f46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830750"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription



Définit la description de la tâche spécifiée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetDescription <Job> <Description>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Description|Texte utilisé pour décrire la tâche.|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère la description de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /SetDescription myDownloadJob "Music Downloads"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)