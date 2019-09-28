---
title: Utilisation de la commande Enable-TransportServer
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 732a021b02193a3bfb5cb573a33879dbecb840b9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363434"
---
# <a name="using-the-enable-transportserver-command"></a>Utilisation de la commande Enable-TransportServer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Active tous les services du serveur de transport.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de transport. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom n’est spécifié, le serveur local est utilisé.|
## <a name="BKMK_examples"></a>Illustre
Pour activer les services sur le serveur, exécutez l’une des opérations suivantes :
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-TransportServer](using-the-disable-transportserver-command.md)
[à l’aide de la commande TransportServer](using-the-get-transportserver-command.md)
 sous-[commande : Set-TransportServer](subcommand-set-transportserver.md)
 sous-[commande : sous-commande Start-TransportServer](subcommand-start-transportserver.md)
[: Stop-TransportServer](subcommand-stop-transportserver.md)
