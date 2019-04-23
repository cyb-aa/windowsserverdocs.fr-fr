---
title: À l’aide de la commande disable-serveur
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b69fcfe0-b744-4794-bc75-2c9218c0ba66
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b229146206c1fbe6ce8b6f585b2ff9b50ae6104
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853030"
---
# <a name="using-the-disable-server-command"></a>À l’aide de la commande disable-serveur



Désactive tous les services pour un serveur de Services de déploiement Windows.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /Disable-Server [/Server:<Server name>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[/ Server :\<nom du serveur >]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|

## <a name="BKMK_examples"></a>Exemples

Pour désactiver le serveur, exécutez une des opérations suivantes :
```
WDSUTIL /Disable-Server
WDSUTIL /Verbose /Disable-Server /Server:MyWDSServer
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

