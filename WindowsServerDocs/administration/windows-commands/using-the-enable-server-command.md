---
title: À l’aide de la commande enable-serveur
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdf03a778a6c646aa79c2f844212b1728c5c73eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852720"
---
# <a name="using-the-enable-server-command"></a>À l’aide de la commande enable-serveur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permet à tous les services des Services de déploiement Windows.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
## <a name="BKMK_examples"></a>Exemples
Pour activer les services sur le serveur, exécutez une des opérations suivantes :
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande get-Server](using-the-get-server-command.md)
[à l’aide du Commande Initialize-Server](using-the-initialize-server-command.md)
[sous-commande : set-Server](subcommand-set-server.md)
[sous-commande : start-Server](subcommand-start-server.md) 
 [ Sous-commande : stop-Server](subcommand-stop-server.md)
[l’Option de serveur uninitialize](the-uninitialize-server-option.md)
