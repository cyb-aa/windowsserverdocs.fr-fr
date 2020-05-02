---
title: Sous-commande Start-Server
description: Rubrique de référence pour la sous-commande Start-Server, qui démarre tous les services d’un serveur des services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2a6de007e62bf3be5544f97b53a4fcc13118985
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721653"
---
# <a name="subcommand-start-server"></a>Sous-commande : Start-Server

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Démarre tous les services d’un serveur des services de déploiement Windows.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur à démarrer. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a>Exemples
Pour démarrer le serveur, tapez l’un des éléments suivants :
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Disable-Server](using-the-disable-server-command.md)à l’aide de la commande[Enable-Server](using-the-enable-server-command.md)
[à](using-the-get-server-command.md)
l’aide de la commande Set-Server à l’aide de la sous-commande de
[commande Initialize-Server](using-the-initialize-server-command.md)[: Set-Server](subcommand-set-server.md)
sous-[commande : Stop-Server](subcommand-stop-server.md)
[l’option Uninitialize-Server](the-uninitialize-server-option.md)
