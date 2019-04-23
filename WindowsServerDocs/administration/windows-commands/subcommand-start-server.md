---
title: Sous-commande start-Server
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a89e3f17aeed7eb3156e28997206517342be109
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852040"
---
# <a name="subcommand-start-server"></a>Sous-commande : start-Server

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

démarre tous les services pour un serveur de Services de déploiement Windows.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur doit être démarré. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
## <a name="BKMK_examples"></a>Exemples
Pour démarrer le serveur, tapez une des opérations suivantes :
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande enable-Server](using-the-enable-server-command.md)
[à l’aide du Get-Server commande](using-the-get-server-command.md)
[à l’aide de la commande Initialize-Server](using-the-initialize-server-command.md)
[sous-commande : set-Server](subcommand-set-server.md) 
 [ Sous-commande : stop-Server](subcommand-stop-server.md)
[l’Option de serveur uninitialize](the-uninitialize-server-option.md)
