---
title: À l’aide de la commande enable-TransportServer
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 40793ac0b9dc7d8b4a80d6a66b55244202aa37d1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834330"
---
# <a name="using-the-enable-transportserver-command"></a>À l’aide de la commande enable-TransportServer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permet à tous les services pour le serveur de Transport.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de Transport. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom n’est spécifié, le serveur local doit être utilisé.|
## <a name="BKMK_examples"></a>Exemples
Pour activer les services sur le serveur, exécutez une des opérations suivantes :
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-TransportServer](using-the-disable-transportserver-command.md)
[à l’aide de la commande get-TransportServer](using-the-get-transportserver-command.md) 
 [Sous-commande : set-TransportServer](subcommand-set-transportserver.md)
[sous-commande : start-TransportServer](subcommand-start-transportserver.md)
[sous-commande : stop-TransportServer](subcommand-stop-transportserver.md)
