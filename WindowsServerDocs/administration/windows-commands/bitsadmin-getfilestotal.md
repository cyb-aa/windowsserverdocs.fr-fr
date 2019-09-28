---
title: bitsadmin getfilestotal
description: La rubrique commandes Windows pour **Bitsadmin getfilestotal** -récupère le nombre de fichiers dans le travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27cf04e8745aeab5cd1f2ce379c8506be642fea2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381607"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



Récupère le nombre de fichiers dans le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère le nombre de fichiers inclus dans le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

# #

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md) Voir aussi