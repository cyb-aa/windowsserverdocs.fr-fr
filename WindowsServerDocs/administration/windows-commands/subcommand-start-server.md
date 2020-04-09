---
title: Sous-commande Start-Server
description: Rubrique relative aux commandes Windows pour la sous-commande Start-Server, qui démarre tous les services d’un serveur des services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc3e0d11348dd6b531671e62139f2ce2c884c5ee
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833742"
---
# <a name="subcommand-start-server"></a>Sous-commande : Start-Server

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Démarre tous les services d’un serveur des services de déploiement Windows.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur à démarrer. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour démarrer le serveur, tapez l’un des éléments suivants :
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande Enable-Server](using-the-enable-server-command.md)
[à l’aide de](using-the-get-server-command.md) la commande Set-Server
[à l’aide de la commande Initialize-server](using-the-initialize-server-command.md)
sous-commande [: set-Server](subcommand-set-server.md)
sous- [commande : Stop-Server](subcommand-stop-server.md)
[l’option Uninitialize-Server](the-uninitialize-server-option.md)
