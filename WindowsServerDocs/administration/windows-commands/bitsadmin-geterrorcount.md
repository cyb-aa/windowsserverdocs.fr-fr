---
title: bitsadmin geterrorcount
description: La rubrique commandes Windows pour **Bitsadmin GetErrorCount** -récupère le nombre de fois où le travail spécifié a généré une erreur temporaire.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e5aa64c0e080e946e84c0bf804527bb00cad70a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381619"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount



Récupère le nombre de fois où le travail spécifié a généré une erreur temporaire.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetErrorCount <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère des informations sur le nombre d’erreurs pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetErrorCount myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)