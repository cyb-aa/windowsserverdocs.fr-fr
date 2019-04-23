---
title: À l’aide de la commande de copie-DriverGroup
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 68d9c6f4ca78991bb4c286042a6172211161dd1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842080"
---
# <a name="using-the-copy-drivergroup-command"></a>À l’aide de la commande de copie-DriverGroup



Duplique un groupe de pilotes existant sur le serveur, y compris les filtres, les packages de pilotes et l’état activé/désactivé.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[/ Server :\<nom du serveur >]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/ DriverGroup :\<Source nom_groupe >|Spécifie le nom du groupe pilote source.|
|/ GroupName :\<nom du nouveau groupe >|Spécifie le nom du nouveau groupe pilote.|

## <a name="BKMK_examples"></a>Exemples

Pour copier un groupe de pilotes, tapez une des opérations suivantes :
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)