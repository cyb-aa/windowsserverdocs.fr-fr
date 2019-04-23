---
title: bitsadmin getproxyusage
description: Rubrique de commandes de Windows pour **bitsadmin getproxyusage** -récupère le paramètre d’utilisation de proxy pour le travail spécifié.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 20ba418b8dfcf3d96d9b20b22e53797a232a13f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863880"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage



Récupère le paramètre de l’utilisation de proxy pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetProxyUsage <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

Les valeurs possibles sont :
-   PRÉCONFIGURATION, utiliser les valeurs par défaut de Internet Explorer du propriétaire.
-   NO_PROXY : n’utilisez pas un serveur proxy.
-   REMPLACER, utilisez une liste de proxy explicite.
-   Détection automatique, détecter automatiquement les paramètres de proxy.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère l’utilisation de proxy pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetProxyUsage myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)