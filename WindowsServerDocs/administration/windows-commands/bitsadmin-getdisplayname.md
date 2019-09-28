---
title: bitsadmin getdisplayname
description: La rubrique commandes Windows pour **Bitsadmin GetDisplayName** -récupère le nom complet du travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 229bd245f9e810fc6aeb856bbfba253b9ab8a9f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381631"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname



Récupère le nom complet du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetDisplayName <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant extrait le nom complet de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetDisplayName myDownloadJob
```
Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)