---
title: Sous-commande Stop-TransportServer
description: Rubrique relative aux commandes Windows pour STOP-TransportServer
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2a2444328a426429c2dce5ceee3272cf1dc814cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370717"
---
# <a name="subcommand-stop-transportserver"></a>Sous-commande : Stop-TransportServer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Arrête tous les services sur un serveur de transport.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de transport. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun serveur de transport n’est spécifié, le serveur local est utilisé.|
## <a name="BKMK_examples"></a>Illustre
Pour arrêter les services, tapez l’un des éléments suivants :
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-TransportServer](using-the-disable-transportserver-command.md)
[à l’aide de la commande Enable-TransportServer](using-the-enable-transportserver-command.md)
[à l’aide de la commande TransportServer](using-the-get-transportserver-command.md)
 sous-[commande : Set-TransportServer](subcommand-set-transportserver.md)
 sous-[commande : Start-TransportServer](subcommand-start-transportserver.md)
