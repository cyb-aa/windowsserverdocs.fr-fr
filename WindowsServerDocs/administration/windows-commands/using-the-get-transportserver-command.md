---
title: Utilisation de la commande TransportServer
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 282b69162cf3550c5bcba3282b60f15072c96ed6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363074"
---
# <a name="using-the-get-transportserver-command"></a>Utilisation de la commande TransportServer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche des informations sur un serveur de transport spécifié.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Show : {config}|Retourne des informations de configuration sur le serveur de transport spécifié.|
## <a name="BKMK_examples"></a>Illustre
Pour afficher des informations sur le serveur, tapez :
```
wdsutil /Get-TransportServer /Show:Config
```
Pour afficher les informations de configuration, tapez :
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-TransportServer](using-the-disable-transportserver-command.md)
[à l’aide de la commande Enable-TransportServer](using-the-enable-transportserver-command.md)
 sous-[commande : Set-TransportServer](subcommand-set-transportserver.md)
 sous-[commande : sous-commande Start-TransportServer](subcommand-start-transportserver.md)
[: Stop-TransportServer](subcommand-stop-transportserver.md)
