---
title: Bitsadmin GetClientCertificate
description: La rubrique commandes Windows pour **Bitsadmin GetClientCertificate** -récupère le certificat client à partir du travail.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 613feafb442f63513d34e9038647c4dbeb278630
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381719"
---
# <a name="bitsadmin-getclientcertificate"></a>Bitsadmin GetClientCertificate



Récupère le certificat client du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetClientCertificate <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère le certificat client pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin / GetClientCertificate myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)