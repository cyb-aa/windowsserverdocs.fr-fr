---
title: Sous-commande Stop-Server
description: Rubrique de référence pour la sous-commande Stop-Server, qui arrête tous les services sur un serveur des services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68cc73ac016e2ffded774567034801e1c11944d1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721633"
---
# <a name="subcommand-stop-server"></a>Sous-commande : Stop-Server

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arrête tous les services sur un serveur des services de déploiement Windows.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a>Exemples
Pour arrêter les services, tapez l’un des éléments suivants :
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Disable-Server](using-the-disable-server-command.md)à l’aide de la commande[Enable-Server](using-the-enable-server-command.md)
[à](using-the-get-server-command.md)
l’aide de la commande Set-Server à l'[aide de la sous-commande Initialize-Server](using-the-initialize-server-command.md)
, commande[: Set-Server](subcommand-set-server.md)
, sous[-commande : Start-Server](subcommand-start-server.md)
[l’option Uninitialize-Server](the-uninitialize-server-option.md)
