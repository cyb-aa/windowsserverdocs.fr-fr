---
title: Sous-commande start-TransportServer
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fdfea020019a45eceac0142160f9d5d4d97b989
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848630"
---
# <a name="subcommand-start-transportserver"></a>Sous-commande : start-TransportServer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

démarre tous les services pour un serveur de Transport.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de Transport. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
## <a name="BKMK_examples"></a>Exemples
Pour démarrer le serveur, tapez une des opérations suivantes :
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-TransportServer](using-the-disable-transportserver-command.md)
[à l’aide de la commande enable-TransportServer](using-the-enable-transportserver-command.md) 
 [ À l’aide de la commande get-TransportServer](using-the-get-transportserver-command.md)
[sous-commande : set-TransportServer](subcommand-set-transportserver.md)
[sous-commande : stop-TransportServer](subcommand-stop-transportserver.md)
