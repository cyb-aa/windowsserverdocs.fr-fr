---
title: Stop-TransportServer sous-commande
description: Rubrique de commandes de Windows pour arrêter-TransportServer
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8f7410a8720337e509325b99863446bd8d19eb26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853450"
---
# <a name="subcommand-stop-transportserver"></a>Sous-commande : stop-TransportServer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arrête tous les services sur un serveur de Transport.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de Transport. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun serveur de Transport est spécifié, le serveur local doit être utilisé.|
## <a name="BKMK_examples"></a>Exemples
Pour arrêter les services, tapez une des opérations suivantes :
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-TransportServer](using-the-disable-transportserver-command.md)
[à l’aide de la commande enable-TransportServer](using-the-enable-transportserver-command.md) 
 [ À l’aide de la commande get-TransportServer](using-the-get-transportserver-command.md)
[sous-commande : set-TransportServer](subcommand-set-transportserver.md)
[sous-commande : start-TransportServer](subcommand-start-transportserver.md)
