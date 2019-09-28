---
title: Utilisation de la commande ImageGroup
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d6531a3b69840a0a4910b2effdd3e349b76edf2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363119"
---
# <a name="using-the-get-imagegroup-command"></a>Utilisation de la commande ImageGroup

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Récupère des informations sur un groupe d’images et les images qu’il contient.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaGroup : <Image group name>|Spécifie le nom du groupe d'images.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/detailed|Retourne les métadonnées d’image pour chaque image. Si ce paramètre n’est pas utilisé, le comportement par défaut consiste à retourner uniquement le nom de l’image, la description et le nom du fichier.|
## <a name="BKMK_examples"></a>Illustre
Pour afficher des informations sur un groupe d’images, tapez :
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Pour afficher des informations telles que des métadonnées, tapez :
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-ImageGroup](using-the-add-imagegroup-command.md)
[à l’aide de la commande AllImageGroups](using-the-get-allimagegroups-command.md)
[à l’aide de la commande Remove-ImageGroup](using-the-remove-imagegroup-command.md)
 sous-[commande : Set-ImageGroup](subcommand-set-imagegroup.md)
