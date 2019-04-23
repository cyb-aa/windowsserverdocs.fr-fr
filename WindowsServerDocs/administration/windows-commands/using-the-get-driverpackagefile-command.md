---
title: À l’aide de la commande get-DriverPackageFile
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed9518fae07745502d01dc0084b7443a1332db83
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859800"
---
# <a name="using-the-get-driverpackagefile-command"></a>À l’aide de la commande get-DriverPackageFile



Affiche des informations sur un package de pilotes, y compris les pilotes et les fichiers qu’il contient.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ InfFile :\<chemin d’accès du fichier Inf >|Spécifie le chemin d’accès et le nom complet du fichier .inf de package de pilotes.|
|[/ Architecture : {x86 | ia64 | x64}]|Spécifie l’architecture du package de pilotes.|
|[/ Afficher : {pilotes | Fichiers | All}]|Indique les informations de package à afficher. Si **/afficher** n’est pas spécifié, la valeur par défaut est de retourner uniquement le pilote les métadonnées du package. **Pilotes** affiche la liste des pilotes dans le package. **Fichiers** affiche la liste des fichiers dans le package. **Tous les** affiche les pilotes et les fichiers.|

## <a name="BKMK_examples"></a>Exemples

Pour afficher des informations sur un fichier de pilote, tapez :
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)