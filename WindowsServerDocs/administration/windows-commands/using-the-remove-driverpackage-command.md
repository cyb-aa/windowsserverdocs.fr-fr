---
title: Utilisation de la commande Remove-DriverPackage
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b201e91-0d44-4e4a-8252-8b0235df1002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 923a86805134c4162b36cdade98c2122b3cb7ccd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362810"
---
# <a name="using-the-remove-driverpackage-command"></a>Utilisation de la commande Remove-DriverPackage

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

supprime un package de pilotes d’un serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /remove-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Paramètres

|        Paramètre        |                                                                            Description                                                                             |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |              Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.              |
| [/DriverPackage :<Name>] |                                                        Spécifie le nom du package de pilotes à supprimer.                                                         |
|    [/PackageId :<ID>]    | Spécifie l’ID des services de déploiement Windows du package de pilotes à supprimer. Vous devez spécifier l’ID si le package de pilotes ne peut pas être identifié de manière unique par son nom. |

## <a name="BKMK_examples"></a>Illustre
Pour afficher des informations sur les images, tapez l’une des options suivantes :
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande Remove-DriverPackages](using-the-remove-driverpackages-command.md)
