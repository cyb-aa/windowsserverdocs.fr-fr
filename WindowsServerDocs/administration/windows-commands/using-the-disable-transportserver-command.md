---
title: À l’aide de la commande disable-TransportServer
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d653aacf225333ac8a8b090ef53fb6826e84ab5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836980"
---
# <a name="using-the-disable-transportserver-command"></a>À l’aide de la commande disable-TransportServer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Désactive tous les services pour un serveur de Transport.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de Transport doit être désactivée. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur de Transport est spécifié, le serveur local doit être utilisé.|
## <a name="BKMK_examples"></a>Exemples
Pour désactiver le serveur, tapez :
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande enable-TransportServer](using-the-enable-transportserver-command.md)
[à l’aide de la commande get-TransportServer](using-the-get-transportserver-command.md) 
 [Sous-commande : set-TransportServer](subcommand-set-transportserver.md)
[sous-commande : start-TransportServer](subcommand-start-transportserver.md)
[sous-commande : stop-TransportServer](subcommand-stop-transportserver.md)
