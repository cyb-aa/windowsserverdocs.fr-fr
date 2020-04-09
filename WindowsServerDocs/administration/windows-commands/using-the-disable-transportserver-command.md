---
title: Disable-TransportServer
description: Rubrique relative aux commandes Windows pour Disable-TransportServer, qui désactive tous les services d’un serveur de transport.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39f930a464364cda680098ef4e7e1081d0995503
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831622"
---
# <a name="disable-transportserver"></a>Disable-TransportServer

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Désactive tous les services d’un serveur de transport.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de transport à désactiver. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur de transport n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour désactiver le serveur, tapez :
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande enable-TransportServer](using-the-enable-transportserver-command.md)
[à l’aide de la commande TransportServer](using-the-get-transportserver-command.md)
sous [-commande : Set-TransportServer](subcommand-set-transportserver.md)
sous- [commande : Start-TransportServer](subcommand-start-transportserver.md)
sous- [commande : Stop-TransportServer](subcommand-stop-transportserver.md)
