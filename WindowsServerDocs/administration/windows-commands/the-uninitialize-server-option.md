---
title: Uninitialize-Server
description: La rubrique commandes Windows pour Uninitialize-Server, qui annule les modifications apportées au serveur lors de la configuration initiale du serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ed912af1ec3bc505e1e7465650a119af979b407
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833112"
---
# <a name="uninitialize-server"></a>Uninitialize-Server

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Annule les modifications apportées au serveur lors de la configuration initiale du serveur. Cela comprend les modifications apportées par l’option **/Initialize-Server** ou le composant logiciel enfichable MMC des services de déploiement Windows. Notez que cette commande réinitialise le serveur à un État non configuré. Cette commande ne modifie pas le contenu du dossier partagé remoteInstall. Au lieu de cela, il réinitialise l’état du serveur afin que vous puissiez réinitialiser le serveur.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour réinitialiser le serveur, tapez l’un des éléments suivants :
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande Enable-Server](using-the-enable-server-command.md)
à l’aide [de la commande](using-the-get-server-command.md) Set-Server
à l' [aide de la commande Initialize-server](using-the-initialize-server-command.md)
sous-commande [: set-Server](subcommand-set-server.md)
sous-commande [: Start-Server](subcommand-start-server.md)
sous- [commande : Stop-Server](subcommand-stop-server.md)
