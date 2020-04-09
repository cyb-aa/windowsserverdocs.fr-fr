---
title: Sous-commande Stop-Server
description: La rubrique commandes Windows pour la sous-commande Stop-Server, qui arrête tous les services sur un serveur des services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2671e7a2c2e5bb542cecd9374ad6364d68ff5bd4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833712"
---
# <a name="subcommand-stop-server"></a>Sous-commande : Stop-Server

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arrête tous les services sur un serveur des services de déploiement Windows.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour arrêter les services, tapez l’un des éléments suivants :
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande Enable-Server](using-the-enable-server-command.md)
[à l’aide de](using-the-get-server-command.md) la commande Set-Server
[à l’aide de la commande Initialize-server](using-the-initialize-server-command.md)
sous-commande [: set-Server](subcommand-set-server.md)
sous- [commande : Start-Server](subcommand-start-server.md)
[l’option Uninitialize-Server](the-uninitialize-server-option.md)
