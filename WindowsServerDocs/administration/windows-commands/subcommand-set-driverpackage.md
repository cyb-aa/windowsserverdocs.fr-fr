---
title: Sous-commande Set-DriverPackage
description: La rubrique commandes Windows pour la sous-commande Set-DriverPackage, qui renomme et/ou active ou désactive un package de pilotes sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68d282b3338e67a33a55481658f55db69930b10e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834016"
---
# <a name="subcommand-set-driverpackage"></a>Sous-commande : Set-DriverPackage

Renomme et/ou active ou désactive un package de pilotes sur un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

### <a name="parameters"></a>Paramètres

|        Paramètre         |                                                                                                                                                                                                               Description                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server :\<nom du serveur >] |                                                                                                                                                 Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                                                                 |
| [/DriverPackage : nom de\<>] |                                                                                                                                                                                       Spécifie le nom actuel du package de pilotes à modifier.                                                                                                                                                                                        |
|    [/PackageId : ID de\<>]    | Spécifie l’ID des services de déploiement Windows du package de pilotes. Vous devez spécifier cette option si le package de pilotes ne peut pas être identifié de manière unique par son nom. Pour trouver cet ID pour un package, cliquez sur le groupe de pilotes dans lequel se trouve le package (ou sur le nœud **tous les packages** ), cliquez avec le bouton droit sur le package, puis cliquez sur **Propriétés**. L’ID de package est indiqué sous l’onglet **général** . Par exemple : {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}. |
|   [/Name :\<nouveau nom >]    |                                                                                                                                                                                              Spécifie le nouveau nom du package de pilotes.                                                                                                                                                                                              |
|      [/Enabled : {Oui      |                                                                                                                                                                                                                   º                                                                                                                                                                                                                    |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour modifier les paramètres d’un package, tapez l’un des éléments suivants :
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)