---
title: L’Option de serveur uninitialize
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73f1ff67331ae41fa0d88cb3a16df5095e0b6d66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873980"
---
# <a name="the-uninitialize-server-option"></a>L’Option de serveur uninitialize

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Annule les modifications apportées au serveur lors de la configuration initiale du serveur. Cela inclut les modifications apportées à l’aide du **/Initialize-Server** option ou la console mmc composant logiciel enfichable Services de déploiement Windows. Notez que cette commande réinitialise le serveur à un état non configuré. Cette commande ne modifie pas le contenu du dossier remoteInstall partagé. Au lieu de cela, il réinitialise l’état du serveur afin que vous pouvez réinitialiser le serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
## <a name="BKMK_examples"></a>Exemples
Pour réinitialiser le serveur, tapez une des opérations suivantes :
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande enable-Server](using-the-enable-server-command.md)
[à l’aide du Get-Server commande](using-the-get-server-command.md)
[à l’aide de la commande Initialize-Server](using-the-initialize-server-command.md)
[sous-commande : set-Server](subcommand-set-server.md) 
 [ Sous-commande : start-Server](subcommand-start-server.md)
[sous-commande : stop-Server](subcommand-stop-server.md)
