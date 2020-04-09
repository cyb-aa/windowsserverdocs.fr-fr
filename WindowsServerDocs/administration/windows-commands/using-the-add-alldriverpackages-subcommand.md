---
title: Utilisation de la sous-commande Add-AllDriverPackages
description: La rubrique commandes Windows pour Add-AllDriverPackages, qui ajoute tous les packages de pilotes stockés dans un dossier à un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc8252339fcae04517c2074c24bbfab44228b779
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832252"
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
|  /FolderPath : chemin d’accès au dossier\<>  |                      Spécifie le chemin d’accès complet au dossier qui contient les fichiers. inf des packages de pilotes.                      |
|   [/Server :\<nom du serveur >]   | Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé. |
|     [/Architecture : {x86      |                                                                 ia64                                                                  |
| [/DriverGroup : nom de groupe\<>] |                             Spécifie le nom du groupe de pilotes auquel les packages doivent être ajoutés.                             |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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