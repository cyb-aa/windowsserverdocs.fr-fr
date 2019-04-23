---
title: Bitsadmin getclientcertificate
description: Rubrique de commandes de Windows pour **bitsadmin getclientcertificate** -récupère le certificat client à partir de la tâche.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 113d733d1deb9fbb1c89231495cb7af668a444d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869040"
---
# <a name="bitsadmin-getclientcertificate"></a>Bitsadmin getclientcertificate



Récupère le certificat client à partir de la tâche.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetClientCertificate <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère le certificat client pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin / GetClientCertificate myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)