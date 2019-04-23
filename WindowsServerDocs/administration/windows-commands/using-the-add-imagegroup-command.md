---
title: À l’aide de la commande add-ImageGroup
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 71050bfecdac4933bfe36f40ce09dae626735664
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829900"
---
# <a name="using-the-add-imagegroup-command"></a>À l’aide de la commande add-ImageGroup

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute un groupe d’images à un serveur de Services de déploiement Windows.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaGroup :<Image group name>|Spécifie le nom du groupe de l’image à ajouter.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n'est spécifié, le serveur local est utilisé.|
## <a name="BKMK_examples"></a>Exemples
Pour ajouter un groupe d’images, tapez une des opérations suivantes :
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:"My Image Group" /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande get-AllImageGroups](using-the-get-allimagegroups-command.md)
[à l’aide de la commande get-ImageGroup](using-the-get-imagegroup-command.md) 
 [ À l’aide de la commande remove-ImageGroup](using-the-remove-imagegroup-command.md)
[sous-commande : set-ImageGroup](subcommand-set-imagegroup.md)
