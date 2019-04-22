---
title: À l’aide de la commande get-TransportServer
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 08aa1273d09ba92de15e13f7bfcc8283ac2fedb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817420"
---
# <a name="using-the-get-transportserver-command"></a>À l’aide de la commande get-TransportServer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur un serveur de Transport spécifié.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
|/ Afficher : {Config}|Retourne les informations de configuration sur le serveur de Transport spécifié.|
## <a name="BKMK_examples"></a>Exemples
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
[à l’aide de la commande enable-TransportServer](using-the-enable-transportserver-command.md) 
 [ Sous-commande : set-TransportServer](subcommand-set-transportserver.md)
[sous-commande : start-TransportServer](subcommand-start-transportserver.md)
[sous-commande : stop-TransportServer](subcommand-stop-transportserver.md)
