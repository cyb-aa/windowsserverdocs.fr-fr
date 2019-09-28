---
title: Utilisation de la commande Remove-DriverGroup
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d22ae4e191c2110a0b8d4cc50c24c2f3ec4a7e60
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362930"
---
# <a name="using-the-remove-drivergroup-command"></a>Utilisation de la commande Remove-DriverGroup



supprime un groupe de pilotes d’un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/DriverGroup : nom de \<Group >|Spécifie le nom du groupe de pilotes à supprimer.|
|[/Server : @no__t-nom 0Server >]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|

## <a name="BKMK_examples"></a>Illustre

Pour supprimer un groupe de pilotes, tapez l’un des éléments suivants :
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)