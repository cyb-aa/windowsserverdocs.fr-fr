---
title: Utilisation de la commande Add-ImageGroup
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e870bd5435e1aa2b155fee880d32c0d784ac398
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363676"
---
# <a name="using-the-add-imagegroup-command"></a>Utilisation de la commande Add-ImageGroup

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Ajoute un groupe d’images à un serveur des services de déploiement Windows.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaGroup : <Image group name>|Spécifie le nom du groupe d’images à ajouter.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n'est spécifié, le serveur local est utilisé.|
## <a name="BKMK_examples"></a>Illustre
Pour ajouter un groupe d’images, tapez l’un des éléments suivants :
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:"My Image Group" /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande AllImageGroups](using-the-get-allimagegroups-command.md)
[à l’aide de la commande ImageGroup](using-the-get-imagegroup-command.md)
[à l’aide de la commande Remove-ImageGroup](using-the-remove-imagegroup-command.md)
 sous-[commande : Set-ImageGroup](subcommand-set-imagegroup.md)
