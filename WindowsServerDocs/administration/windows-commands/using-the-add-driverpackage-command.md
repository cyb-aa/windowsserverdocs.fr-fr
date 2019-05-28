---
title: À l’aide de la commande add-DriverPackage
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7e96391ee8dce0b77f00f51d7cb78ff9b8bf242
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564654"
---
# <a name="using-the-add-driverpackage-command"></a>À l’aide de la commande add-DriverPackage



Ajoute un package de pilotes au serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|InfFile :\<chemin d’accès du fichier Inf >|Spécifie le chemin d’accès complet du fichier .inf à ajouter.|
|/ Server :\<nom du serveur >|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/ Architecture : {x86 | ia64 | x64}|Spécifie l’architecture du package de pilotes.|
|[/ DriverGroup :\<nom du groupe >]|Spécifie le nom du groupe pilote auquel le package doit être ajouté.|
|[/ Nom :\<nom convivial >]|Indique le nom convivial pour le package de pilotes.|

## <a name="BKMK_examples"></a>Exemples

Pour ajouter un package de pilotes, tapez une des opérations suivantes :
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:"C:\Temp\Display.inf"
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:"C:\Temp\Display.inf" /Architecture:x86 /DriverGroup:x86Drivers /Name:"Display Driver"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

