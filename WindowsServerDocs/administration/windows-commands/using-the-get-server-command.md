---
title: Serveur de récupération
description: Rubrique relative aux commandes Windows pour obtenir-Server, qui récupère des informations à partir du serveur des services de déploiement Windows spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65903dc89730eb9d1da23be31ecc1909daece9c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830842"
---
# <a name="get-server"></a>Serveur de récupération

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère les informations du serveur des services de déploiement Windows spécifié.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Show : {config &#124; images &#124; tout}|Spécifie le type d’informations à retourner.<p>-   **config** retourne des informations de configuration.<br />les **images** de -   renvoient des informations sur les groupes d’images, les images de démarrage et les images d’installation.<br />-   **toutes les** informations de configuration et les informations d’image.|
|/detailed|Vous pouvez utiliser cette option avec **/Show : images** ou **/Show : All** pour indiquer que toutes les métadonnées d’image de chaque image doivent être retournées. Si l’option **/detailed** n’est pas utilisée, le comportement par défaut consiste à retourner le nom de l’image, la description et le nom du fichier.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour afficher des informations sur le serveur, tapez :
```
wdsutil /Get-Server /Show:Config
```
Pour afficher des informations détaillées sur le serveur, tapez :
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande Enable-Server](using-the-enable-server-command.md)
[à l’aide de la commande Initialize-Server](using-the-initialize-server-command.md)
sous-commande [: set-server](subcommand-set-server.md)
sous- [commande : Start-](subcommand-start-server.md) Server
[subcommand : Stop-Server](subcommand-stop-server.md)
[l’option Uninitialize-Server](the-uninitialize-server-option.md)
