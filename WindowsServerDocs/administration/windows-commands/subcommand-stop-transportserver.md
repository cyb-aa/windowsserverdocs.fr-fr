---
title: Sous-commande Stop-TransportServer
description: Rubrique de référence pour STOP-TransportServer
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4321ec991b2c20911f992e4c3c38e5c9cfa5f165
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721619"
---
# <a name="subcommand-stop-transportserver"></a>Sous-commande : Stop-TransportServer

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arrête tous les services sur un serveur de transport.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de transport. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun serveur de transport n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a><a name="BKMK_examples"></a>Illustre
Pour arrêter les services, tapez l’un des éléments suivants :
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Disable-TransportServer](using-the-disable-transportserver-command.md)à l’aide de la
[commande Enable-TransportServer](using-the-enable-transportserver-command.md)[à l’aide de la](using-the-get-transportserver-command.md)
sous-commande de commande TransportServer[: Set-TransportServer](subcommand-set-transportserver.md)
sous-[commande : Start-TransportServer](subcommand-start-transportserver.md)
