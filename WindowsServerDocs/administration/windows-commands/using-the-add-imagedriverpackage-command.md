---
title: Utilisation de la commande Add-ImageDriverPackage
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1069b7c63e8b3bbc28fd900e8c869afc8abc03bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363728"
---
# <a name="using-the-add-imagedriverpackage-command"></a>Utilisation de la commande Add-ImageDriverPackage

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Ajoute un package de pilotes qui se trouve dans le magasin de pilotes à une image de démarrage existante sur le serveur. La version de l’image doit être Windows 7 ou Windows Server 2008 R2 ou version ultérieure.
## <a name="syntax"></a>Syntaxe
```
wdsutil /add-ImageDriverPackage [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
```
```
[/Filename:<File name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Paramètres

|                 Paramètre                  |                                                                                                                                                                                                            Description                                                                                                                                                                                                             |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           [/Server : <Server name>           |                                                                                                                                               Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                                                                |
|             média : <Image name>             |                                                                                                                                                                                       Spécifie le nom de l’image à laquelle ajouter le pilote.                                                                                                                                                                                        |
|               MediaType : démarrage               |                                                                                                                                                                Spécifie le type d’image auquel ajouter le pilote. Les packages de pilotes peuvent uniquement être ajoutés aux images de démarrage.                                                                                                                                                                 |
| /Architecture : {x86 &#124; ia64 &#124; x64} |                                                                                                       Spécifie l’architecture de l’image de démarrage. Étant donné qu’il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, vous devez spécifier l’architecture pour vous assurer que l’image correcte est utilisée.                                                                                                        |
|           /Filename : <File name>]           |                                                                                                                                                        Spécifie le nom du fichier. Si l’image ne peut pas être identifiée de manière unique par son nom, le nom de fichier doit être spécifié.                                                                                                                                                        |
|           [/DriverPackage : <Name>           |                                                                                                                                                                                   Spécifie le nom du package de pilotes à ajouter à l’image.                                                                                                                                                                                    |
|             [/PackageId : <ID>]              | Spécifie l’ID des services de déploiement Windows du package de pilotes. Vous devez spécifier cette option si le package de pilotes ne peut pas être identifié de manière unique par son nom. Pour trouver l’ID de package, cliquez sur le groupe de pilotes dans lequel se trouve le package (ou sur le nœud **tous les packages** ), cliquez avec le bouton droit sur le package, puis cliquez sur **Propriétés**. L’ID de package est indiqué sous l’onglet **général** . Par exemple : {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}. |

## <a name="BKMK_examples"></a>Illustre
Pour ajouter un package de pilotes à une image de démarrage, tapez l’une des options suivantes :
```
wdsutil /add-ImageDriverPackagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande Add-ImageDriverPackages](using-the-add-imagedriverpackages-command.md)
