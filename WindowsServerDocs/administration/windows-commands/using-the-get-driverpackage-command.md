---
title: DriverPackage
description: Rubrique de référence pour la commande DriverPackage, qui affiche des informations sur un package de pilotes sur le serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4fc6bc327b46f8219a7c40fa47e85cc94b6fc749
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719943"
---
# <a name="get-driverpackage"></a>DriverPackage

Affiche des informations sur un package de pilotes sur le serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>Paramètres

|        Paramètre         |                                                                           Description                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server :\<nom du serveur>] |              Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.               |
| [/DriverPackage :\<nom>] |                                                        Spécifie le nom du package de pilotes à afficher.                                                         |
|    [/PackageId :\<ID>]    | Spécifie l’ID des services de déploiement Windows du package de pilotes à afficher. Vous devez spécifier l’ID si le package de pilotes ne peut pas être identifié de manière unique par son nom. |
|     [/Show : {drivers     |                                                                              Fichiers                                                                               |

## <a name="examples"></a>Exemples

Pour afficher des informations sur un package de pilotes, tapez l’un des éléments suivants :
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)