---
title: Utilisation de la commande DriverPackage
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3d31d9a02454b0f7fca06b28a4df27174f7b02e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363190"
---
# <a name="using-the-get-driverpackage-command"></a>Utilisation de la commande DriverPackage



Affiche des informations sur un package de pilotes sur le serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Paramètres

|        Paramètre         |                                                                           Description                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server :\<nom du serveur >] |              Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.               |
| [/DriverPackage : nom de\<>] |                                                        Spécifie le nom du package de pilotes à afficher.                                                         |
|    [/PackageId : ID de\<>]    | Spécifie l’ID des services de déploiement Windows du package de pilotes à afficher. Vous devez spécifier l’ID si le package de pilotes ne peut pas être identifié de manière unique par son nom. |
|     [/Show : {drivers     |                                                                              Fichiers                                                                               |

## <a name="BKMK_examples"></a>Illustre

Pour afficher des informations sur un package de pilotes, tapez l’un des éléments suivants :
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)