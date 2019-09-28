---
title: bitsadmin getnotifyinterface
description: Rubrique relative aux commandes Windows pour **Bitsadmin getnotifyinterface** -détermine si un autre programme a inscrit une interface de rappel com pour le travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 826e13cf8a3e54935ceb5a72ff82647cacfc3be5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381468"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

Détermine si un autre programme a inscrit une interface de rappel COM (l’interface de notification) pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetNotifyInterface <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Affiche inscrit ou désinscrit.

> [!NOTE]
> Il n’est pas possible de déterminer le programme qui a inscrit l’interface de rappel.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère l’interface Notify pour la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyInterface myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)