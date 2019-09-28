---
title: Utilisation de la commande AllImages
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5122a5660031d503795715c0005b404f910d6626
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363500"
---
# <a name="using-the-get-allimages-command"></a>Utilisation de la commande AllImages

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Récupère des informations sur toutes les images sur un serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Show : {boot &#124; install &#124; LegacyRis &#124; All}|le**démarrage** de -    retourne uniquement les images de démarrage.<br />l'**installation** de -    retourne les images d’installation et les informations sur les groupes d’images qui les contiennent.<br />-   **LegacyRis** retourne uniquement les images des services d’installation à distance (RIS).<br />-    retourne**toutes les** informations sur l’image de démarrage, les informations sur l’image d’installation (y compris les informations sur les groupes d’images) et les informations sur l’image RIS.|
|/detailed|Indique que toutes les métadonnées d’image de chaque image doivent être retournées. Si cette option n’est pas utilisée, le comportement par défaut consiste à retourner uniquement le nom de l’image, la description et le nom du fichier.|
## <a name="BKMK_examples"></a>Illustre
Pour afficher des informations sur les images, tapez l’une des options suivantes :
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-image](using-the-add-image-command.md)
[à l’aide de la commande copy-image](using-the-copy-image-command.md)
[à l’aide de la commande Export-image](using-the-export-image-command.md)
[à l’aide de la commande Remove-Image](using-the-remove-image-command.md)
[à l’aide de l’option Commande Replace-image](using-the-replace-image-command.md)1 sous-[commande : Set-image](subcommand-set-image.md)
