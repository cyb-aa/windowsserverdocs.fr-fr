---
title: À l’aide de la commande get-DriverPackage
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6574177f1f0a8ead0cc2fa596380eb2c980f8d1f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888520"
---
# <a name="using-the-get-driverpackage-command"></a>À l’aide de la commande get-DriverPackage



Affiche des informations sur un package de pilotes sur le serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[/ Server :\<nom du serveur >]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/ DriverPackage :\<nom >]|Spécifie le nom du package de pilotes à afficher.|
|[/PackageId:\<ID>]|Spécifie l’ID de Services de déploiement Windows du package de pilotes s’affiche. Vous devez spécifier l’ID si le package de pilotes ne peut pas être identifié de manière unique par son nom.|
|[/ Afficher : {pilotes | Fichiers | All}]|Indique les informations à afficher (si spécifié). Si **/afficher** n’est pas spécifié, la valeur par défaut est de retourner uniquement le pilote les métadonnées du package. **Pilotes** affiche tous les pilotes dans le package. **Fichiers** affiche la liste des fichiers dans le package. **Tous les** affiche les pilotes, les fichiers et les métadonnées.|

## <a name="BKMK_examples"></a>Exemples

Pour afficher des informations sur un package de pilotes, tapez une des opérations suivantes :
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)