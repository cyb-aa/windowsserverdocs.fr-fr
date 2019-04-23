---
title: bitsadmin getnotifyinterface
description: Rubrique de commandes de Windows pour **bitsadmin getnotifyinterface** -détermine si un autre programme a inscrit une interface de rappel COM pour le travail spécifié.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8316721a20cc477f9e8e15fc57b5d1c861da3ff4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868040"
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
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

Affiche les inscrit ou non inscrit.

> [!NOTE]
> Il n’est pas possible de déterminer le programme qui inscrit l’interface de rappel.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère l’interface de notification pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyInterface myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)