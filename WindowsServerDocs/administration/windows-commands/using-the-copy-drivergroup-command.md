---
title: Copy-DriverGroup
description: Rubrique de référence pour Copy-DriverGroup, qui duplique un groupe de pilotes existant sur le serveur, y compris les filtres, les packages de pilotes et l’état activé/désactivé.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc157e9ef6d07a45efe2a19221fb3a046b2f65c1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721013"
---
# <a name="copy-drivergroup"></a>Copy-DriverGroup

Duplique un groupe de pilotes existant sur le serveur, y compris les filtres, les packages de pilotes et l’état activé/désactivé.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[/Server :\<nom du serveur>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/DriverGroup :\<nom du groupe source>|Spécifie le nom du groupe de pilotes source.|
|/GroupName :\<nouveau nom de groupe>|Spécifie le nom du nouveau groupe de pilotes.|

## <a name="examples"></a>Exemples

Pour copier un groupe de pilotes, tapez l’un des éléments suivants :
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)