---
title: AllImages
description: Rubrique de référence pour la fonction AllImages, qui récupère des informations sur toutes les images sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1f32a1789b22d04b7b61979d0ea49d91f0cf157
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720017"
---
# <a name="get-allimages"></a>AllImages

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère des informations sur toutes les images sur un serveur.

## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Show : {Boot &#124; installer &#124; LegacyRis &#124; tout}|-   Le **démarrage** retourne uniquement les images de démarrage.<br />-   **Install** retourne les images d’installation et les informations sur les groupes d’images qui les contiennent.<br />-   **LegacyRis** retourne uniquement les images des services d’installation à distance (RIS).<br />-   **Tout** retourne des informations sur l’image de démarrage, des informations sur l’image d’installation (y compris des informations sur les groupes d’images) et des informations sur l’image RIS.|
|/detailed|Indique que toutes les métadonnées d’image de chaque image doivent être retournées. Si cette option n’est pas utilisée, le comportement par défaut consiste à retourner uniquement le nom de l’image, la description et le nom du fichier.|
## <a name="examples"></a>Exemples
Pour afficher des informations sur les images, tapez l’une des options suivantes :
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l’aide[de la commande Add-image](using-the-add-image-command.md)
[à l’aide de la commande](using-the-copy-image-command.md)
copy-image à l’aide de la commande[Export-](using-the-export-image-command.md)
image à l’aide de la commande[Remove-Image](using-the-remove-image-command.md)
à l’aide de la sous-commande de
[commande Replace-image](using-the-replace-image-command.md)[: Set-image](subcommand-set-image.md)
