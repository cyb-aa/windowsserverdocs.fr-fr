---
title: bitsadmin gettype
description: Rubrique de commandes de Windows pour **bitsadmin gettype** -récupère le type de tâche de la tâche spécifiée.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff0118f14acbf4e9f37c02e660bd9c7f6e8d0f70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879430"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype



Récupère le type de travail de la tâche spécifiée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetType <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

Le type peut être le téléchargement, télécharger, de chargement-réponse ou inconnu.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère le type de tâche pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetType myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)