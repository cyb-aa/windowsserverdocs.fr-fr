---
title: Utilisation de la commande Copy-DriverGroup
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c08ce616c9b0e2bf79c7f13f922e27d7f7f7ca62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363592"
---
# <a name="using-the-copy-drivergroup-command"></a>Utilisation de la commande Copy-DriverGroup



Duplique un groupe de pilotes existant sur le serveur, y compris les filtres, les packages de pilotes et l’état activé/désactivé.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[/Server : @no__t-nom 0Server >]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/DriverGroup : \<Source nom du groupe >|Spécifie le nom du groupe de pilotes source.|
|/GroupName : \<New nom du groupe >|Spécifie le nom du nouveau groupe de pilotes.|

## <a name="BKMK_examples"></a>Illustre

Pour copier un groupe de pilotes, tapez l’un des éléments suivants :
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)