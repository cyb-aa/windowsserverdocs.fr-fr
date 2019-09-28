---
title: Utilisation de la commande de serveur de récupération
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c7e0ee4529858b16cdc63ea1d6d358a190b8b1a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392119"
---
# <a name="using-the-get-server-command"></a>Utilisation de la commande de serveur de récupération

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Récupère les informations du serveur des services de déploiement Windows spécifié.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Show : {config &#124; images &#124; tout}|Spécifie le type d’informations à retourner.<br /><br />-   **config** retourne des informations de configuration.<br />les**images** -    renvoient des informations sur les groupes d’images, les images de démarrage et les images d’installation.<br />-    retourne**toutes les** informations de configuration et les informations d’image.|
|/detailed|Vous pouvez utiliser cette option avec **/Show : images** ou **/Show : All** pour indiquer que toutes les métadonnées d’image de chaque image doivent être retournées. Si l’option **/detailed** n’est pas utilisée, le comportement par défaut consiste à retourner le nom de l’image, la description et le nom du fichier.|
## <a name="BKMK_examples"></a>Illustre
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
[à l’aide de la commande Enable-Server](using-the-enable-server-command.md)
[à l’aide de la commande Initialize-Server](using-the-initialize-server-command.md)
 sous-[commande : Set-Server](subcommand-set-server.md)
[ Sous-commande : Start-Server](subcommand-start-server.md)1 sous-[commande : Stop-Server](subcommand-stop-server.md)3[l’option Uninitialize-Server](the-uninitialize-server-option.md)
