---
title: Add-DriverGroupPackage
description: Rubrique relative aux commandes Windows pour Add-DriverGroupPackage, qui ajoute un package de pilotes à un groupe de pilotes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6753baeb03b99ee149250d41844469a5008f5ecd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832072"
---
# <a name="add-drivergrouppackage"></a>Add-DriverGroupPackage

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute un package de pilotes à un groupe de pilotes.

## <a name="syntax"></a>Syntaxe
```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>Paramètres

|         Paramètre         |                                                                                                                                               Description                                                                                                                                               |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup :<Group Name> |                                                                                                                                 Spécifie le nom du groupe de pilotes.                                                                                                                                 |
|   /Server :<Server name>   |                                                                                  Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                  |
|   /DriverPackage :<Name>   |                                                                      Spécifie le nom du package de pilotes à ajouter au groupe. Vous devez spécifier cette option si le package de pilotes ne peut pas être identifié de manière unique par son nom.                                                                       |
|      /PackageId :<ID>      | Spécifie l’ID d’un package. Pour trouver l’ID de package, cliquez sur le groupe de pilotes dans lequel se trouve le package (ou sur le nœud **tous les packages** ), cliquez avec le bouton droit sur le package, puis cliquez sur **Propriétés**. L’ID de package est indiqué sous l’onglet **général** , par exemple : **{DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}** . |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour ajouter un package de pilotes, tapez l’un des éléments suivants :
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-DriverGroupPackages](using-the-add-drivergrouppackages-command.md)
[à l’aide de la commande Add-DriverPackage](using-the-add-driverpackage-command.md)
[à l’aide de la sous-commande Add-AllDriverPackages](using-the-add-alldriverpackages-subcommand.md)
