---
title: À l’aide de la commande remove-DriverPackage
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 217ff23b8724464670520d0b2d5b196df5a4af47
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440301"
---
# <a name="using-the-remove-driverpackage-command"></a>À l’aide de la commande remove-DriverPackage

> S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime un package de pilotes à partir d’un serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /remove-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Paramètres

|        Paramètre        |                                                                            Description                                                                             |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |              Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si un nom de serveur n’est pas spécifié, le serveur local est utilisé.              |
| [/ DriverPackage :<Name>] |                                                        Spécifie le nom du package de pilotes à supprimer.                                                         |
|    [/PackageId:<ID>]    | Spécifie l’ID de Services de déploiement Windows du package de pilotes à supprimer. Vous devez spécifier l’ID si le package de pilotes ne peut pas être identifié de manière unique par son nom. |

## <a name="BKMK_examples"></a>Exemples
Pour afficher des informations sur les images, tapez une des opérations suivantes :
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande remove-DriverPackages](using-the-remove-driverpackages-command.md)
