---
title: Add-ImageGroup
description: Rubrique relative aux commandes Windows pour Add-ImageGroup, qui ajoute un groupe d’images à un serveur des services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f12ed27ca1a809ec34dbefbc4ff7288ff194a83e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831892"
---
# <a name="add-imagegroup"></a>Add-ImageGroup

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute un groupe d’images à un serveur des services de déploiement Windows.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaGroup :<Image group name>|Spécifie le nom du groupe d’images à ajouter.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n'est spécifié, le serveur local est utilisé.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour ajouter un groupe d’images, tapez l’un des éléments suivants :
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande AllImageGroups](using-the-get-allimagegroups-command.md)
[à l’aide de la commande ImageGroup](using-the-get-imagegroup-command.md)
[à l’aide de la commande Remove-ImageGroup](using-the-remove-imagegroup-command.md)
sous [-commande : Set-ImageGroup](subcommand-set-imagegroup.md)
