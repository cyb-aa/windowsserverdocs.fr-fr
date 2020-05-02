---
title: Uninitialize-Server
description: Rubrique de référence pour Uninitialize-Server, qui annule les modifications apportées au serveur lors de la configuration initiale du serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d7747a44172b7382bd22a7d48ccc717a89ccacb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721395"
---
# <a name="uninitialize-server"></a>Uninitialize-Server

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Annule les modifications apportées au serveur lors de la configuration initiale du serveur. Cela comprend les modifications apportées par l’option **/Initialize-Server** ou le composant logiciel enfichable MMC des services de déploiement Windows. Notez que cette commande réinitialise le serveur à un État non configuré. Cette commande ne modifie pas le contenu du dossier partagé remoteInstall. Au lieu de cela, il réinitialise l’état du serveur afin que vous puissiez réinitialiser le serveur.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a>Exemples
Pour réinitialiser le serveur, tapez l’un des éléments suivants :
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande[à l’aide de la commande](using-the-disable-server-command.md)
Disable-Server à l’aide de la commande[Enable-Server](using-the-enable-server-command.md)
[à](using-the-get-server-command.md)
l’aide de la commande Set-Server à l’aide de la sous-commande de
[commande Initialize-Server](using-the-initialize-server-command.md)[: Set-Server](subcommand-set-server.md)
sous-commande :[Start-](subcommand-start-server.md)
Server, sous-[commande : Stop-Server](subcommand-stop-server.md)
