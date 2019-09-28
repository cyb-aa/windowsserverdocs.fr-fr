---
title: Utilisation de la commande New-DiscoverImage
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ede9fbbb-0bba-4309-8c21-3cc13e1dc3cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b17777eb4d8541ce5669ee6becbd51e2b9da236
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362991"
---
# <a name="using-the-new-discoverimage-command"></a>Utilisation de la commande New-DiscoverImage



Crée une image de découverte à partir d’une image de démarrage existante. Les images de découverte sont des images de démarrage qui forcent le démarrage du programme Setup. exe en mode services de déploiement Windows, puis découvrent un serveur des services de déploiement Windows. En général, ces images sont utilisées pour déployer des images sur des ordinateurs qui ne sont pas en capacité de démarrer dans PXE. Pour plus d’informations, consultez Création d’images ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /New-DiscoverImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
         /FilePath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
         [/WDSServer:<Server name>]
         [/Overwrite:{Yes | No | Append}]
```

## <a name="parameters"></a>Paramètres

|        Paramètre         |                                                                                                                                                                                                                                                                                                                                                                                                                       Description                                                                                                                                                                                                                                                                                                                                                                                                                       |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server : @no__t-nom 0Server >] |                                                                                                                                                                                                                                                                                                                                     Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                                                                                                                                                                                                                                                     |
|   /Image : nom @no__t 0Image >   |                                                                                                                                                                                                                                                                                                                                                                                                      Spécifie le nom de l’image de démarrage source.                                                                                                                                                                                                                                                                                                                                                                                                       |
|    /Architecture : {x86    |                                                                                                                                                                                                                                                                                                                                                                                                                          ia64                                                                                                                                                                                                                                                                                                                                                                                                                           |
| [/Filename : @no__t-nom 0File >] |                                                                                                                                                                                                                                                                                                                                                                         Si l’image ne peut pas être identifiée de manière unique par son nom, vous devez utiliser cette option pour spécifier le nom du fichier.                                                                                                                                                                                                                                                                                                                                                                          |
|    /DestinationImage     | Spécifie les paramètres de l’image de destination. Vous pouvez spécifier les paramètres à l’aide des options suivantes :</br>-/FilePath. : < chemin d’accès et nom de fichier >-définit le chemin de fichier complet de la nouvelle image.</br>-[/Name : \<Name >]-définit le nom d’affichage de l’image. Si aucun nom complet n’est spécifié, le nom complet de l’image source est utilisé.</br>-[/Description : \<Description >]-définit la description de l’image.</br>-[/WDSServer : \<Server nom >] : spécifie le nom du serveur que tous les clients qui démarrent à partir de l’image spécifiée doivent contacter pour télécharger l’image d’installation. Par défaut, tous les clients qui démarrent cette image découvrent un serveur des services de déploiement Windows valide. L’utilisation de cette option contourne la fonctionnalité de découverte et force le client amorcé à contacter le serveur spécifié.</br>-[/Overwrite : {Oui |

## <a name="BKMK_examples"></a>Illustre

Pour créer une image de découverte à partir de l’image de démarrage et la nommer WinPEDiscover. wim, tapez :
```
WDSUTIL /New-DiscoverImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPEDiscover.wim"
```
Pour créer une image de découverte à partir de l’image de démarrage et la nommer WinPEDiscover. wim avec les paramètres spécifiés, tapez :
```
WDSUTIL /Verbose /Progress /New-DiscoverImage /Server:MyWDSServer
/Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim /DestinationImage /FilePath:"\\Server\Share\WinPEDiscover.wim" 
/Name:"New WinPE image" /Description:"WinPE image for WDS Client discovery" /Overwrite:No
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)