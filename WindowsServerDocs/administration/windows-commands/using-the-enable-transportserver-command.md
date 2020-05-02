---
title: Enable-TransportServer
description: Rubrique de référence pour Enable-TransportServer, qui active tous les services du serveur de transport.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50cc381b1c178628be7d135868027b4f37787cdd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720922"
---
# <a name="enable-transportserver"></a>Enable-TransportServer

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active tous les services du serveur de transport.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de transport. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a>Exemples
Pour activer les services sur le serveur, exécutez l’une des opérations suivantes :
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Disable-TransportServer](using-the-disable-transportserver-command.md)[à l’aide de la](using-the-get-transportserver-command.md)
sous-commande de commande TransportServer[: Set-TransportServer](subcommand-set-transportserver.md)
sous-[commande : Start-TransportServer](subcommand-start-transportserver.md)
sous-[commande : Stop-TransportServer](subcommand-stop-transportserver.md)
