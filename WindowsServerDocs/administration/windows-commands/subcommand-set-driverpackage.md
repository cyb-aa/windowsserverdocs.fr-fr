---
title: Sous-commande Set-DriverPackage
description: Rubrique de référence pour la sous-commande Set-DriverPackage, qui renomme et/ou active ou désactive un package de pilotes sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 40a812e785df6820da404a8951af6731cced15d3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721722"
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
| [/Server :\<nom du serveur>] |                                                                                                                                                 Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                                                                 |
| [/DriverPackage :\<nom>] |                                                                                                                                                                                       Spécifie le nom actuel du package de pilotes à modifier.                                                                                                                                                                                        |
|    [/PackageId :\<ID>]    | Spécifie l’ID des services de déploiement Windows du package de pilotes. Vous devez spécifier cette option si le package de pilotes ne peut pas être identifié de manière unique par son nom. Pour trouver cet ID pour un package, cliquez sur le groupe de pilotes dans lequel se trouve le package (ou sur le nœud **tous les packages** ), cliquez avec le bouton droit sur le package, puis cliquez sur **Propriétés**. L’ID de package est indiqué sous l’onglet **général** . Par exemple : {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}. |
|   [/Name :\<nouveau nom>]    |                                                                                                                                                                                              Spécifie le nouveau nom du package de pilotes.                                                                                                                                                                                              |
|      [/Enabled : {Oui      |                                                                                                                                                                                                                   º                                                                                                                                                                                                                    |

## <a name="examples"></a>Exemples

Pour modifier les paramètres d’un package, tapez l’un des éléments suivants :
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)