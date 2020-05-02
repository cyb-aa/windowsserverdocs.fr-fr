---
title: Sous-commande Start-TransportServer
description: Rubrique de référence pour la sous-commande Start-TransportServer, qui démarre tous les services d’un serveur de transport.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 92bd68421883c49ec29dfb78f06121bff880b01e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721636"
---
# <a name="subcommand-start-transportserver"></a>Sous-commande : Start-TransportServer

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Démarre tous les services d’un serveur de transport.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de transport. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a>Exemples
Pour démarrer le serveur, tapez l’un des éléments suivants :
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Disable-TransportServer](using-the-disable-transportserver-command.md)à l’aide de la
[commande Enable-TransportServer](using-the-enable-transportserver-command.md)[à l’aide de la](using-the-get-transportserver-command.md)
sous-commande de commande TransportServer[: Set-TransportServer](subcommand-set-transportserver.md)
sous-[commande : Stop-TransportServer](subcommand-stop-transportserver.md)
