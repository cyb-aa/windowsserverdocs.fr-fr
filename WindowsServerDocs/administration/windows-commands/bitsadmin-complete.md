---
title: bitsadmin complete
description: La rubrique commandes Windows pour **Bitsadmin terminé** -termine la tâche. Les fichiers téléchargés ne sont pas disponibles tant que vous n’utilisez pas ce commutateur.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d5a1dc5dbbf2d5b3207b5423f338e0caf4412599
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381817"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Termine le travail. Les fichiers téléchargés ne sont pas disponibles tant que vous n’utilisez pas ce commutateur. Utilisez ce commutateur après que le travail passe à l’état transféré. Dans le cas contraire, seuls les fichiers qui ont été transférés avec succès sont disponibles.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /complete <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

Lorsque l’état du travail est transféré, le service BITS a transféré tous les fichiers du travail. Toutefois, les fichiers ne sont pas disponibles tant que vous n’utilisez pas le commutateur **/Complete** Si plusieurs travaux utilisent *myDownloadJob* comme nom, vous devez remplacer *MYDOWNLOADJOB* par le GUID du travail pour identifier le travail de façon unique.
```
C:\>bitsadmin /complete myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)