---
title: bitsadmin getnoprogresstimeout
description: Rubrique de commandes de Windows pour **bitsadmin getnoprogresstimeout** -récupère la longueur de la durée, en secondes, pendant laquelle le service tente de transférer le fichier après une erreur temporaire se produit.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9563b68b8012a49471b56e3b8f2fbd60d1c69756
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850800"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout



Récupère la longueur de la durée, en secondes, pendant laquelle le service tente de transférer le fichier après qu’une erreur temporaire se produit.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetNoProgressTimeout <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère la valeur de délai d’expiration de progression du travail *myDownloadJob*.
```
C:\>bitsadmin /GetNoProgressTimeout myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)