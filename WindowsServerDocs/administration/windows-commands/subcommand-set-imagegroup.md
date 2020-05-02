---
title: Sous-commande Set-ImageGroup
description: Rubrique de référence pour la sous-commande Set-ImageGroup, qui modifie les attributs d’un groupe d’images.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 429a8fee5b0236d264eb421f110219a1bc037368
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721696"
---
# <a name="subcommand-set-imagegroup"></a>Sous-commande : Set-ImageGroup

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie les attributs d’un groupe d’images.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaGroup:<Image group name>|Spécifie le nom du groupe d'images.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. S’il n’est pas spécifié, le serveur local est utilisé.|
|[/Name :<New image group name>]|Spécifie le nouveau nom du groupe d’images.|
|[/Security :<SDDL>]|Spécifie le nouveau descripteur de sécurité du groupe d’images, au format SDDL (Security Descriptor Definition Language).|
## <a name="examples"></a>Exemples
Pour définir le nom d’un groupe d’images, tapez :
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:New Image Group Name
```
Pour spécifier différents paramètres pour un groupe d’images, tapez :
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:New Image Group Name 
/Security:O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande[à l’aide de la commande Add-ImageGroup](using-the-add-imagegroup-command.md)
[à](using-the-get-allimagegroups-command.md)
l’aide de la commande-AllImageGroups à l’aide de la commande-[ImageGroup](using-the-get-imagegroup-command.md)
avec[la commande Remove-ImageGroup](using-the-remove-imagegroup-command.md)
