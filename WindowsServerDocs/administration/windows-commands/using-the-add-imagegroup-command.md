---
title: Add-ImageGroup
description: Rubrique de référence pour Add-ImageGroup, qui ajoute un groupe d’images à un serveur des services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b08042ac6b33c0ccfe0b66bb0fec70805d55d75f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721050"
---
# <a name="add-imagegroup"></a>Add-ImageGroup

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute un groupe d’images à un serveur des services de déploiement Windows.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaGroup:<Image group name>|Spécifie le nom du groupe d’images à ajouter.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n'est spécifié, le serveur local est utilisé.|
## <a name="examples"></a>Exemples
Pour ajouter un groupe d’images, tapez l’un des éléments suivants :
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande[à l’aide de la commande](using-the-get-allimagegroups-command.md)
AllImageGroups à l’aide de la[commande](using-the-get-imagegroup-command.md)
Set-ImageGroup à l’aide de la sous-commande de
[commande Remove-ImageGroup](using-the-remove-imagegroup-command.md)[: Set-ImageGroup](subcommand-set-imagegroup.md)
