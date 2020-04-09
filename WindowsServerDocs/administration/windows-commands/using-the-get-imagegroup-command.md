---
title: ImageGroup
description: Rubrique relative aux commandes Windows pour la commande ImageGroup, qui récupère des informations sur un groupe d’images et les images qu’il contient.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0066e5d52c1d10b1f78ea627ee7a476bfd98f19d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830952"
---
# <a name="get-imagegroup"></a>ImageGroup

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère des informations sur un groupe d’images et les images qu’il contient.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaGroup :<Image group name>|Spécifie le nom du groupe d'images.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/detailed|Retourne les métadonnées d’image pour chaque image. Si ce paramètre n’est pas utilisé, le comportement par défaut consiste à retourner uniquement le nom de l’image, la description et le nom du fichier.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour afficher des informations sur un groupe d’images, tapez :
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Pour afficher des informations telles que des métadonnées, tapez :
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-ImageGroup](using-the-add-imagegroup-command.md)
[à l’aide de la commande AllImageGroups](using-the-get-allimagegroups-command.md)
[à l’aide de la commande Remove-ImageGroup](using-the-remove-imagegroup-command.md)
sous [-commande : Set-ImageGroup](subcommand-set-imagegroup.md)
