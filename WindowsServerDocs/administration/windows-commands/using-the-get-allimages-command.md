---
title: À l’aide de la commande get-AllImages
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 57b81dd3dd3a24876c4401e80d08130ed5243888
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872560"
---
# <a name="using-the-get-allimages-command"></a>À l’aide de la commande get-AllImages

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère des informations sur toutes les images sur un serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
|/ Show : {démarrage &#124; installer &#124; LegacyRis &#124; tous les}|-   **Démarrage** retourne uniquement les images de démarrage.<br />-   **Installer** retourne installer des images, ainsi que des informations sur les groupes de l’image qui les contiennent.<br />-   **LegacyRis** retourne uniquement les images Installation Services (RIS) à distance.<br />-   **Tous les** retourne des informations sur l’image, les informations d’image install (y compris les informations sur les groupes de l’image) et les informations relatives aux images RIS de démarrage.|
|[/detailed]|Indique que toutes les métadonnées d’image de chaque image doivent être retournées. Si cette option n’est pas utilisée, le comportement par défaut consiste à retourner uniquement le nom de l’image, description et nom de fichier.|
## <a name="BKMK_examples"></a>Exemples
Pour afficher des informations sur les images, tapez une des opérations suivantes :
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-Image](using-the-add-image-command.md)
[à l’aide de la commande de copie-Image](using-the-copy-image-command.md)
[à l’aide du Commande export-Image](using-the-export-image-command.md)
[à l’aide de la commande remove-Image](using-the-remove-image-command.md)
[à l’aide de l’Image de remplacement commande](using-the-replace-image-command.md) 
 [Sous-commande : set-Image](subcommand-set-image.md)
