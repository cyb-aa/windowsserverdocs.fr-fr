---
title: bitsadmin getproxyusage
description: La rubrique commandes Windows pour **Bitsadmin getproxyusage** -récupère le paramètre d’utilisation du proxy pour le travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea9a22f4fb35af3436d02d9f23b62ce0888a26b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381290"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage



Récupère le paramètre d’utilisation du proxy pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetProxyUsage <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Les valeurs possibles sont :
-   Préconfiguration : utilisez les paramètres par défaut d’Internet Explorer du propriétaire.
-   NO_PROXY : n’utilisez pas de serveur proxy.
-   OVERRIDE : utilisez une liste de proxys explicite.
-   DÉTECTION automatique : détecte automatiquement les paramètres du proxy.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère l’utilisation du proxy pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetProxyUsage myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)