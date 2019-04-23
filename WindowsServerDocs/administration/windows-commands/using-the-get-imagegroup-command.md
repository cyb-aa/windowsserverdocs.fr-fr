---
title: À l’aide de la commande get-ImageGroup
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9dcb76155dc1044730673ed46a53cad57441a246
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862600"
---
# <a name="using-the-get-imagegroup-command"></a>À l’aide de la commande get-ImageGroup

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère des informations sur un groupe d’images et les images qu’il contient.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaGroup :<Image group name>|Spécifie le nom du groupe d'images.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
|[/detailed]|Retourne les métadonnées d’image pour chaque image. Si ce paramètre n’est pas utilisé, le comportement par défaut consiste à retourner uniquement le nom de l’image, description et nom de fichier.|
## <a name="BKMK_examples"></a>Exemples
Pour afficher des informations sur un groupe d’images, tapez :
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Pour afficher plus d’informations, y compris les métadonnées, tapez :
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-ImageGroup](using-the-add-imagegroup-command.md)
[à l’aide de la commande get-AllImageGroups](using-the-get-allimagegroups-command.md) 
 [ À l’aide de la commande remove-ImageGroup](using-the-remove-imagegroup-command.md)
[sous-commande : set-ImageGroup](subcommand-set-imagegroup.md)
