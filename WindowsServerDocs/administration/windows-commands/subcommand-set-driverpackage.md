---
title: Sous-commande Set-DriverPackage
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65751cf6e03baa87c7734b318a26111652bee0a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370820"
---
# <a name="subcommand-set-driverpackage"></a>Sous-commande : Set-DriverPackage



Renomme et/ou active ou désactive un package de pilotes sur un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

## <a name="parameters"></a>Paramètres

|        Paramètre         |                                                                                                                                                                                                               Description                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server :\<nom du serveur >] |                                                                                                                                                 Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                                                                 |
| [/DriverPackage : nom de\<>] |                                                                                                                                                                                       Spécifie le nom actuel du package de pilotes à modifier.                                                                                                                                                                                        |
|    [/PackageId : ID de\<>]    | Spécifie l’ID des services de déploiement Windows du package de pilotes. Vous devez spécifier cette option si le package de pilotes ne peut pas être identifié de manière unique par son nom. Pour trouver cet ID pour un package, cliquez sur le groupe de pilotes dans lequel se trouve le package (ou sur le nœud **tous les packages** ), cliquez avec le bouton droit sur le package, puis cliquez sur **Propriétés**. L’ID de package est indiqué sous l’onglet **général** . Par exemple : {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}. |
|   [/Name :\<nouveau nom >]    |                                                                                                                                                                                              Spécifie le nouveau nom du package de pilotes.                                                                                                                                                                                              |
|      [/Enabled : {Oui      |                                                                                                                                                                                                                   º                                                                                                                                                                                                                    |

## <a name="BKMK_examples"></a>Illustre

Pour modifier les paramètres d’un package, tapez l’un des éléments suivants :
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)