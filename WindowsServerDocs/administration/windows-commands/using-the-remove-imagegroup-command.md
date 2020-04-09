---
title: Remove-ImageGroup
description: Rubrique relative aux commandes Windows pour Remove-ImageGroup, qui supprime un groupe d’images d’un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8406959037a958ea6d61b8e8145317635955e6d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830352"
---
# <a name="using-the-remove-imagegroup-command"></a>Utilisation de la commande Remove-ImageGroup

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime un groupe d’images d’un serveur.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaGroup :<Image group name>|Spécifie le nom du groupe d’images à supprimer|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour supprimer le groupe d’images, tapez l’un des éléments suivants :
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:My Image Group /Server:MyWDSServer 
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
[Utilisation de la commande Add-ImageGroup](using-the-add-imagegroup-command.md)  
[Utilisation de la commande AllImageGroups](using-the-get-allimagegroups-command.md)  
[Utilisation de la commande ImageGroup](using-the-get-imagegroup-command.md)  
[Sous-commande : Set-ImageGroup](subcommand-set-imagegroup.md)  
