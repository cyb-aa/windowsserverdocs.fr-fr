---
title: Add-DriverGroupPackage
description: Rubrique de référence pour Add-DriverGroupPackage, qui ajoute un package de pilotes à un groupe de pilotes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4baf4f16740e65c432cc09ca24270ab479346ac2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721113"
---
# <a name="add-drivergrouppackage"></a>Add-DriverGroupPackage

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute un package de pilotes à un groupe de pilotes.

## <a name="syntax"></a>Syntaxe
```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>Paramètres

|         Paramètre         |                                                                                                                                               Description                                                                                                                                               |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:<Group Name> |                                                                                                                                 Spécifie le nom du groupe de pilotes.                                                                                                                                 |
|   Serveurs<Server name>   |                                                                                  Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                  |
|   /DriverPackage:<Name>   |                                                                      Spécifie le nom du package de pilotes à ajouter au groupe. Vous devez spécifier cette option si le package de pilotes ne peut pas être identifié de manière unique par son nom.                                                                       |
|      PackageId<ID>      | Spécifie l’ID d’un package. Pour trouver l’ID de package, cliquez sur le groupe de pilotes dans lequel se trouve le package (ou sur le nœud **tous les packages** ), cliquez avec le bouton droit sur le package, puis cliquez sur **Propriétés**. L’ID de package est indiqué sous l’onglet **général** , par exemple : **{DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}**. |

## <a name="examples"></a>Exemples
Pour ajouter un package de pilotes, tapez l’un des éléments suivants :
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Add-DriverGroupPackages](using-the-add-drivergrouppackages-command.md)
à l’aide de la[commande Add-DriverPackage](using-the-add-driverpackage-command.md)[à l’aide de la sous-commande Add-AllDriverPackages](using-the-add-alldriverpackages-subcommand.md)
