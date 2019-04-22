---
title: Sous-commande DriverPackage de jeu
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0513d3de2416f93f69b1e1cc286c38b12be94d15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818820"
---
# <a name="subcommand-set-driverpackage"></a>Sous-commande : set-DriverPackage



Renomme et/ou Active ou désactive un package de pilotes sur un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[/ Server :\<nom du serveur >]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si un nom de serveur n’est pas spécifié, le serveur local est utilisé.|
|[/ DriverPackage :\<nom >]|Spécifie le nom actuel du package de pilotes à modifier.|
|[/PackageId:\<ID>]|Spécifie l’ID de Services de déploiement Windows du package de pilotes. Vous devez spécifier cette option si le package de pilotes ne peut pas être identifié de manière unique par son nom. Pour rechercher cet ID pour un package, cliquez sur le groupe de pilotes se trouve le package (ou le **tous les Packages** nœud), cliquez sur le package, puis cliquez sur **propriétés**. L’ID de Package est répertorié dans le **général** onglet. Par exemple : {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}.|
|[/ Nom :\<nouveau nom >]|Spécifie le nouveau nom pour le package de pilotes.|
|[/ Enabled : {Oui | No}|Active ou désactive le package.|

## <a name="BKMK_examples"></a>Exemples

Pour modifier les paramètres sur un package, tapez une des opérations suivantes :
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)