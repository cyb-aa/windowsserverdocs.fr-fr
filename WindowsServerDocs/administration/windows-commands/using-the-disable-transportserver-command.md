---
title: Disable-TransportServer
description: Rubrique de référence relative à Disable-TransportServer, qui désactive tous les services d’un serveur de transport.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81ae150b4f8e4de577e377a2d10a7a69675adac7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720960"
---
# <a name="disable-transportserver"></a>Disable-TransportServer

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Désactive tous les services d’un serveur de transport.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de transport à désactiver. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur de transport n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a>Exemples
Pour désactiver le serveur, tapez :
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Enable-TransportServer](using-the-enable-transportserver-command.md)[à l’aide de la](using-the-get-transportserver-command.md)
sous-commande de commande TransportServer[: Set-TransportServer](subcommand-set-transportserver.md)
sous-[commande : Start-TransportServer](subcommand-start-transportserver.md)
sous-[commande : Stop-TransportServer](subcommand-stop-transportserver.md)
