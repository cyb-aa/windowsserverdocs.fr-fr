---
title: bitsadmin complete
description: Rubrique de commandes de Windows pour **bitsadmin complète** -a terminé son travail. Les fichiers téléchargés ne sont pas disponibles pour vous jusqu'à ce que vous utilisez ce commutateur.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 561585da370f7e69aa3b83b3ddd7579bfc658a21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817320"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Termine la tâche. Les fichiers téléchargés ne sont pas disponibles pour vous jusqu'à ce que vous utilisez ce commutateur. Utilisez ce commutateur lorsque le travail à l’état transférée. Sinon, seuls les fichiers ont été transférés sont disponibles.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /complete <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

Lorsque l’état du travail est transféré, BITS a transféré tous les fichiers dans le travail. Toutefois, les fichiers ne sont pas disponibles tant que vous utilisez le **/ complète** basculer. Si vous utilisent plusieurs travaux *myDownloadJob* comme leur nom, vous devez remplacer *myDownloadJob* avec le GUID du travail pour identifier de façon unique le travail.
```
C:\>bitsadmin /complete myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)