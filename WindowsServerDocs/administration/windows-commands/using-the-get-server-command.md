---
title: Serveur de récupération
description: Rubrique de référence pour obtenir-Server, qui récupère des informations à partir du serveur des services de déploiement Windows spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a760371797af8eb95da386a3a5b9dbb0dcf7ba3c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719736"
---
# <a name="get-server"></a>Serveur de récupération

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère les informations du serveur des services de déploiement Windows spécifié.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Show : {config &#124; images &#124; tout}|Spécifie le type d’informations à retourner.<p>-   La **configuration retourne des** informations de configuration.<br />-   **Images** retourne des informations sur les groupes d’images, les images de démarrage et les images d’installation.<br />-   **Toutes les** informations de configuration et les informations d’image sont retournées.|
|/detailed|Vous pouvez utiliser cette option avec **/Show : images** ou **/Show : All** pour indiquer que toutes les métadonnées d’image de chaque image doivent être retournées. Si l’option **/detailed** n’est pas utilisée, le comportement par défaut consiste à retourner le nom de l’image, la description et le nom du fichier.|
## <a name="examples"></a>Exemples
Pour afficher des informations sur le serveur, tapez :
```
wdsutil /Get-Server /Show:Config
```
Pour afficher des informations détaillées sur le serveur, tapez :
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Disable-Server](using-the-disable-server-command.md)à l’aide de la


[commande Enable-Server](using-the-enable-server-command.md)à l’aide de la sous-commande
de[commande Initialize-Server](using-the-initialize-server-command.md):[Set-Server](subcommand-set-server.md), sous-commande[: Start-Server,](subcommand-start-server.md)sous[-commande : Stop-Server](subcommand-stop-server.md)
[l’option Uninitialize-Server](the-uninitialize-server-option.md)
