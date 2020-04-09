---
title: Sous-commande Stop-TransportServer
description: Rubrique relative aux commandes Windows pour STOP-TransportServer
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44d62332307ffda4dcfa6af286c7b95cb12423dc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833702"
---
# <a name="subcommand-stop-transportserver"></a>Sous-commande : Stop-TransportServer

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-TransportServer](using-the-disable-transportserver-command.md)
[à l’aide de la commande Enable-TransportServer](using-the-enable-transportserver-command.md)
[à l’aide de la commande TransportServer](using-the-get-transportserver-command.md)
sous- [commande : Set-TransportServer](subcommand-set-transportserver.md)
sous- [commande : Start-TransportServer](subcommand-start-transportserver.md)
