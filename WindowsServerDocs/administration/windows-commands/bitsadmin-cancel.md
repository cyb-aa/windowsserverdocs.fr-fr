---
title: bitsadmin cancel
description: Rubrique de commandes de Windows pour **bitsadmin Annuler** - supprime le travail de la file d’attente de transfert et supprime tous les fichiers temporaires associés au travail.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0a4d1e2d6e4fd66cb525316f236d070fcd72d73f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814070"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Supprime le travail de la file d’attente de transfert et supprime tous les fichiers temporaires associés au travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /cancel <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant supprime le *myDownloadJob* travail à partir de la file d’attente de transfert.
```
C:\>bitsadmin /cancel myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)