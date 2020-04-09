---
title: AllImageGroups
description: La rubrique commandes Windows pour la commande AllImageGroups, qui récupère des informations sur tous les groupes d’images sur un serveur et sur toutes les images de ces groupes d’images.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19285422612f8be34d39e6152fcf0300e1919b8a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831324"
---
# <a name="get-allimagegroups"></a>AllImageGroups

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour afficher des informations sur les groupes d’images, tapez l’un des éléments suivants :
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-ImageGroup](using-the-add-imagegroup-command.md)
[à l’aide de la commande ImageGroup](using-the-get-imagegroup-command.md)
[à l’aide de la commande Remove-ImageGroup](using-the-remove-imagegroup-command.md)
sous [-commande : Set-ImageGroup](subcommand-set-imagegroup.md)
