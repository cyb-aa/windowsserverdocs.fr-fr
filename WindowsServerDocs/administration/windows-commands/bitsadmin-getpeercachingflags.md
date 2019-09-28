---
title: Bitsadmin getpeercachingflags
description: La rubrique commandes Windows pour **Bitsadmin getpeercachingflags** -récupère les indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et desservis aux homologues, et si bits peut télécharger du contenu pour le travail à partir d’homologues.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b86214b5289a59e8db2ecff065ab3b8cd17007e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381438"
---
# <a name="bitsadmin-getpeercachingflags"></a>Bitsadmin getpeercachingflags

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Récupère les indicateurs qui déterminent si les fichiers du travail peuvent être mis en cache et desservis aux homologues, et si BITS peut télécharger du contenu pour le travail à partir de pairs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|Tâche|Nom complet ou GUID du travail|

## <a name="BKMK_examples"></a>Illustre
L’exemple suivant récupère les indicateurs du travail nommé *myJob*.

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)


