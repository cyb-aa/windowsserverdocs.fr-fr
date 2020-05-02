---
title: ImageGroup
description: Rubrique de référence pour la fonction ImageGroup, qui récupère des informations sur un groupe d’images et les images qu’il contient.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30a87085cb935f95a209ffdd78ecf2b9fb45dc15
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719900"
---
# <a name="get-imagegroup"></a>ImageGroup

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère des informations sur un groupe d’images et les images qu’il contient.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaGroup:<Image group name>|Spécifie le nom du groupe d'images.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/detailed|Retourne les métadonnées d’image pour chaque image. Si ce paramètre n’est pas utilisé, le comportement par défaut consiste à retourner uniquement le nom de l’image, la description et le nom du fichier.|
## <a name="examples"></a>Exemples
Pour afficher des informations sur un groupe d’images, tapez :
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Pour afficher des informations telles que des métadonnées, tapez :
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Add-ImageGroup](using-the-add-imagegroup-command.md)à l’aide[de la commande](using-the-get-allimagegroups-command.md)
Set-AllImageGroups à l’aide de la sous-commande de[commande](using-the-remove-imagegroup-command.md)
Remove-ImageGroup[: Set-ImageGroup](subcommand-set-imagegroup.md)
