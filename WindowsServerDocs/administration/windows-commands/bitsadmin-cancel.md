---
title: bitsadmin cancel
description: Rubrique relative aux commandes Windows pour **Bitsadmin annuler** -supprime le travail de la file d’attente de transfert et supprime tous les fichiers temporaires associés à ce travail.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 77e46d787359af43a37faba5d844bfec09730454
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381804"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Supprime le travail de la file d’attente de transfert et supprime tous les fichiers temporaires associés à ce travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /cancel <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant supprime la tâche *myDownloadJob* de la file d’attente de transfert.
```
C:\>bitsadmin /cancel myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)