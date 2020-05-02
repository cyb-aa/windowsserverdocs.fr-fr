---
title: TransportServer
description: Rubrique de référence pour la fonction TransportServer, qui affiche des informations sur un serveur de transport spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82ab5f901240f964bd22e7fb8053ed95b1c6fe51
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719722"
---
# <a name="get-transportserver"></a>TransportServer

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur un serveur de transport spécifié.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Show : {config}|Retourne des informations de configuration sur le serveur de transport spécifié.|
## <a name="examples"></a>Exemples
Pour afficher des informations sur le serveur, tapez :
```
wdsutil /Get-TransportServer /Show:Config
```
Pour afficher les informations de configuration, tapez :
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Disable-TransportServer](using-the-disable-transportserver-command.md)[à l’aide de la sous-commande Enable-TransportServer](using-the-enable-transportserver-command.md)
[: Set-TransportServer](subcommand-set-transportserver.md)
sous-[commande : Start-TransportServer](subcommand-start-transportserver.md)
sous-[commande : Stop-TransportServer](subcommand-stop-transportserver.md)
