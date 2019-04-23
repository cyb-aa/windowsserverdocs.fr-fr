---
title: À l’aide de la commande get-serveur
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da8bf0fc6e31bd8d0079933f1d7c529c4fe96f42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870980"
---
# <a name="using-the-get-server-command"></a>À l’aide de la commande get-serveur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère des informations à partir du serveur de Services de déploiement Windows spécifié.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/ Afficher : {Config &#124; Images &#124; tous les}|Spécifie le type d’informations à retourner.<br /><br />-   **Config** retourne des informations de configuration.<br />-   **Images** retourne des informations sur les groupes d’images, les images de démarrage et les images d’installation.<br />-   **Tous les** retourne des informations de configuration et des informations sur l’image.|
|[/detailed]|Vous pouvez utiliser cette option avec **/Show : images** ou **/Show : all** pour indiquer que toutes les métadonnées d’image de chaque image doivent être retournées. Si le **/ détaillées** option n’est pas utilisée, le comportement par défaut consiste à retourner le nom de l’image, la description et le nom de fichier.|
## <a name="BKMK_examples"></a>Exemples
Pour afficher des informations sur le serveur, tapez :
```
wdsutil /Get-Server /Show:Config
```
Pour afficher des informations détaillées sur le serveur, tapez :
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande enable-Server](using-the-enable-server-command.md)
[à l’aide du Commande Initialize-Server](using-the-initialize-server-command.md)
[sous-commande : set-Server](subcommand-set-server.md)
[sous-commande : start-Server](subcommand-start-server.md) 
 [ Sous-commande : stop-Server](subcommand-stop-server.md)
[l’Option de serveur uninitialize](the-uninitialize-server-option.md)
