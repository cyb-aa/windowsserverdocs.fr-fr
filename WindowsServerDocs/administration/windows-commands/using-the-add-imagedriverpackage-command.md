---
title: À l’aide de la commande add-ImageDriverPackage
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: dc675ac202f87703074f78c45b1f264f6105c324
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843740"
---
# <a name="using-the-add-imagedriverpackage-command"></a>À l’aide de la commande add-ImageDriverPackage

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute un package de pilotes qui se trouve dans le magasin de pilotes à une image de démarrage sur le serveur. La version de l’image doit être Windows 7 ou Windows Server 2008 R2 ou version ultérieure.
## <a name="syntax"></a>Syntaxe
```
wdsutil /add-ImageDriverPackage [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
```
```
[/Filename:<File name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/ Server :<Server name>|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
média :<Image name>|Spécifie le nom de l’image à ajouter au pilote.|
mediatype:Boot|Spécifie le type d’image à ajouter au pilote. Packages de pilotes peuvent uniquement être ajoutés aux images de démarrage.|
|/ Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image de démarrage. Comme il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, vous devez spécifier l’architecture pour garantir que l’image correcte est utilisée.|
|/ Filename :<File name>]|Spécifie le nom du fichier. Si l’image ne peut pas être identifié de manière unique par son nom, le nom de fichier doit être spécifié.|
|[/DriverPackage:<Name>|Spécifie le nom du package de pilotes à ajouter à l’image.|
|[/PackageId:<ID>]|Spécifie l’ID de Services de déploiement Windows du package de pilotes. Vous devez spécifier cette option si le package de pilotes ne peut pas être identifié de manière unique par son nom. Pour rechercher l’ID de Package, cliquez sur le groupe de pilotes se trouve le package (ou le **tous les Packages** nœud), cliquez sur le package, puis cliquez sur **propriétés**. L’ID de Package est répertorié dans le **général** onglet. Par exemple : {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}.|
## <a name="BKMK_examples"></a>Exemples
Pour ajouter un package de pilotes à une image de démarrage, tapez une des opérations suivantes :
```
wdsutil /add-ImageDriverPackagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-ImageDriverPackages](using-the-add-imagedriverpackages-command.md)
