---
title: À l’aide de la sous-commande add-AllDriverPackages
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c7a9e95ebd36209d5729f81b7eae9e2660b3606
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890690"
---
# <a name="using-the-add-alldriverpackages-subcommand"></a>À l’aide de la sous-commande add-AllDriverPackages



Ajoute tous les packages de pilotes qui sont stockés dans un dossier sur un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ FolderPath :\<chemin d’accès de dossier >|Spécifie le chemin d’accès complet au dossier qui contient les fichiers .inf pour les packages de pilotes.|
|[/ Server :\<nom du serveur >]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/ Architecture : {x86 | ia64 | x64}]|Spécifie l’architecture des packages de pilotes à ajouter. Packages de pilotes pour les autres architectures sont ignorés.|
|[/ DriverGroup :\<nom du groupe >]|Spécifie le nom du groupe pilote à laquelle les packages doivent être ajoutés.|

## <a name="BKMK_examples"></a>Exemples

Pour ajouter des packages de pilotes, tapez une des opérations suivantes :
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers" /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers\Printers" /DriverGroup:"Printer Drivers"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[Add-WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)