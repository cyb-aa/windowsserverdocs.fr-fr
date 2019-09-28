---
title: bitsadmin gettype
description: La rubrique commandes Windows pour **Bitsadmin GetType** -récupère le type de tâche du travail spécifié.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ca46cb813809621f4fa79b3265198206729a392c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381338"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype



Récupère le type de tâche du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetType <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Le type peut être télécharger, télécharger, Télécharger-répondre ou inconnu.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère le type de tâche pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetType myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)