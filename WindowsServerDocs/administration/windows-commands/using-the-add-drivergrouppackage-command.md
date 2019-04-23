---
title: À l’aide de la commande add-DriverGroupPackage
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b32d72a1317683e4c1bbeb2d6101d1315b69148e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862640"
---
# <a name="using-the-add-drivergrouppackage-command"></a>À l’aide de la commande add-DriverGroupPackage

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute un package de pilotes à un groupe de pilotes.
## <a name="syntax"></a>Syntaxe
```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/ DriverGroup :<Group Name>|Spécifie le nom du groupe pilote.|
|/ Server :<Server name>|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/ DriverPackage :<Name>|Spécifie le nom du package de pilotes à ajouter au groupe. Vous devez spécifier cette option si le package de pilotes ne peut pas être identifié de manière unique par son nom.|
|/ PackageId :<ID>|Spécifie l’ID d’un package. Pour rechercher l’ID de Package, cliquez sur le groupe de pilotes se trouve le package (ou le **tous les Packages** nœud), cliquez sur le package, puis cliquez sur **propriétés**. L’ID de Package est répertorié dans le **général** onglet, par exemple : **{DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}**.|
## <a name="BKMK_examples"></a>Exemples
Pour ajouter un package de pilotes, tapez une des opérations suivantes :
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-DriverGroupPackages](using-the-add-drivergrouppackages-command.md)
[à l’aide de la commande add-DriverPackage](using-the-add-driverpackage-command.md) 
 [à l’aide de la sous-commande add-AllDriverPackages](using-the-add-alldriverpackages-subcommand.md)
