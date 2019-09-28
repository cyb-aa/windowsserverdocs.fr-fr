---
title: Utilisation de la commande Disable-TransportServer
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7531f8a638ac8fabdad08cc0134dbc63873505de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363458"
---
# <a name="using-the-disable-transportserver-command"></a>Utilisation de la commande Disable-TransportServer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Désactive tous les services d’un serveur de transport.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur de transport à désactiver. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur de transport n’est spécifié, le serveur local est utilisé.|
## <a name="BKMK_examples"></a>Illustre
Pour désactiver le serveur, tapez :
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande enable-TransportServer](using-the-enable-transportserver-command.md)
[à l’aide de la commande TransportServer](using-the-get-transportserver-command.md)
 sous-[commande : Set-TransportServer](subcommand-set-transportserver.md)
 sous-[commande : sous-commande Start-TransportServer](subcommand-start-transportserver.md)
[: Stop-TransportServer](subcommand-stop-transportserver.md)
