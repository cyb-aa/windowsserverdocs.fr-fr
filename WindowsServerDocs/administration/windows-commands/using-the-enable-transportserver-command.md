---
title: Enable-TransportServer
description: Rubrique relative aux commandes Windows pour Enable-TransportServer, qui active tous les services du serveur de transport.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ee5f1206633fb47c4a4b066c099fbf9c7020fb6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831482"
---
# <a name="enable-transportserver"></a>Enable-TransportServer

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active tous les services du serveur de transport.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de transport. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour activer les services sur le serveur, exécutez l’une des opérations suivantes :
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-TransportServer](using-the-disable-transportserver-command.md)
[à l’aide de la commande TransportServer](using-the-get-transportserver-command.md)
sous [-commande : Set-TransportServer](subcommand-set-transportserver.md)
sous- [commande : Start-TransportServer](subcommand-start-transportserver.md)
sous- [commande : Stop-TransportServer](subcommand-stop-transportserver.md)
