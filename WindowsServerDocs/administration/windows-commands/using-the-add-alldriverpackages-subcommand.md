---
title: Utilisation de la sous-commande Add-AllDriverPackages
description: Rubrique de référence pour Add-AllDriverPackages, qui ajoute tous les packages de pilotes stockés dans un dossier à un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 31daa8fc3e3304dba5079672ea4619fd085dd74f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721158"
---
# <a name="add-alldriverpackages"></a>Add-AllDriverPackages

Ajoute tous les packages de pilotes stockés dans un dossier à un serveur.

## <a name="syntax"></a>Syntaxe

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

### <a name="parameters"></a>Paramètres

|          Paramètre           |                                                              Description                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  /FolderPath :\<chemin d’accès au dossier>  |                      Spécifie le chemin d’accès complet au dossier qui contient les fichiers. inf des packages de pilotes.                      |
|   [/Server :\<nom du serveur>]   | Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé. |
|     [/Architecture : {x86      |                                                                 ia64                                                                  |
| [/DriverGroup :\<nom de groupe>] |                             Spécifie le nom du groupe de pilotes auquel les packages doivent être ajoutés.                             |

## <a name="examples"></a>Exemples

Pour ajouter des packages de pilotes, tapez l’un des éléments suivants :
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers\Printers /DriverGroup:Printer Drivers
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Add-WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)