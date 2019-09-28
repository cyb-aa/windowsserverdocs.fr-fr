---
title: Utilisation de la sous-commande Add-AllDriverPackages
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d8290a95dd53718b200d10b6804d312abe95e257
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363888"
---
# <a name="using-the-add-alldriverpackages-subcommand"></a>Utilisation de la sous-commande Add-AllDriverPackages



Ajoute tous les packages de pilotes stockés dans un dossier à un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

## <a name="parameters"></a>Paramètres

|          Paramètre           |                                                              Description                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  /FolderPath : chemin d’accès \<Folder >  |                      Spécifie le chemin d’accès complet au dossier qui contient les fichiers. inf des packages de pilotes.                      |
|   [/Server : @no__t-nom 0Server >]   | Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé. |
|     [/Architecture : {x86      |                                                                 ia64                                                                  |
| [/DriverGroup : @no__t > 0Group] |                             Spécifie le nom du groupe de pilotes auquel les packages doivent être ajoutés.                             |

## <a name="BKMK_examples"></a>Illustre

Pour ajouter des packages de pilotes, tapez l’un des éléments suivants :
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers" /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers\Printers" /DriverGroup:"Printer Drivers"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Add-WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)