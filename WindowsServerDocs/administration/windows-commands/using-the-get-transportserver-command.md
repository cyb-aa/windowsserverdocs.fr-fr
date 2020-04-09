---
title: TransportServer
description: Rubrique relative aux commandes Windows pour la commande TransportServer, qui affiche des informations sur un serveur de transport spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 549eb32536bda201a81e5f40ef408359fae39f4b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830812"
---
# <a name="get-transportserver"></a>TransportServer

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur un serveur de transport spécifié.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Show : {config}|Retourne des informations de configuration sur le serveur de transport spécifié.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour afficher des informations sur le serveur, tapez :
```
wdsutil /Get-TransportServer /Show:Config
```
Pour afficher les informations de configuration, tapez :
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-TransportServer](using-the-disable-transportserver-command.md)
[à l’aide de la commande Enable-TransportServer](using-the-enable-transportserver-command.md)
sous [-commande : Set-TransportServer](subcommand-set-transportserver.md)
sous- [commande : Start-TransportServer](subcommand-start-transportserver.md)
sous- [commande : Stop-TransportServer](subcommand-stop-transportserver.md)
