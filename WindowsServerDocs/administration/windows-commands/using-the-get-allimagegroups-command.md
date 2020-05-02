---
title: AllImageGroups
description: Rubrique de référence pour la fonction AllImageGroups, qui récupère des informations sur tous les groupes d’images sur un serveur et sur toutes les images de ces groupes d’images.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 204618955e91f1c9c9659d37ac3dfe2a01897c51
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720026"
---
# <a name="get-allimagegroups"></a>AllImageGroups

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère des informations sur tous les groupes d’images sur un serveur et sur toutes les images de ces groupes d’images.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/detailed|Retourne les métadonnées de l’image à partir de chaque image. Si ce paramètre n’est pas utilisé, le comportement par défaut consiste à retourner uniquement le nom de l’image, la description et le nom de fichier de chaque image.|
## <a name="examples"></a>Exemples
Pour afficher des informations sur les groupes d’images, tapez l’un des éléments suivants :
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Add-ImageGroup](using-the-add-imagegroup-command.md)à l’aide[de la commande](using-the-get-imagegroup-command.md)
Set-ImageGroup à l’aide de la sous-commande de[commande](using-the-remove-imagegroup-command.md)
Remove-ImageGroup[: Set-ImageGroup](subcommand-set-imagegroup.md)
