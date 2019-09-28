---
title: Utilisation de la commande AllImageGroups
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54e302dca5014d084c7277154eb491f9e33a536b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363309"
---
# <a name="using-the-get-allimagegroups-command"></a>Utilisation de la commande AllImageGroups

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Récupère des informations sur tous les groupes d’images sur un serveur et sur toutes les images de ces groupes d’images.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/detailed|Retourne les métadonnées de l’image à partir de chaque image. Si ce paramètre n’est pas utilisé, le comportement par défaut consiste à retourner uniquement le nom de l’image, la description et le nom de fichier de chaque image.|
## <a name="BKMK_examples"></a>Illustre
Pour afficher des informations sur les groupes d’images, tapez l’un des éléments suivants :
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-ImageGroup](using-the-add-imagegroup-command.md)
[à l’aide de la commande ImageGroup](using-the-get-imagegroup-command.md)
[à l’aide de la commande Remove-ImageGroup](using-the-remove-imagegroup-command.md)
 sous-[commande : Set-ImageGroup](subcommand-set-imagegroup.md)
