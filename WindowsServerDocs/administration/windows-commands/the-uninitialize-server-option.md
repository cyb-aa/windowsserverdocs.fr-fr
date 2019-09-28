---
title: Option Uninitialize-Server
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5c63e09738871c5b74c1b564a83c35ad28f4fa80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385589"
---
# <a name="the-uninitialize-server-option"></a>Option Uninitialize-Server

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

annule les modifications apportées au serveur lors de la configuration initiale du serveur. Cela comprend les modifications apportées par l’option **/Initialize-Server** ou le composant logiciel enfichable MMC des services de déploiement Windows. Notez que cette commande réinitialise le serveur à un État non configuré. Cette commande ne modifie pas le contenu du dossier partagé remoteInstall. Au lieu de cela, il réinitialise l’état du serveur afin que vous puissiez réinitialiser le serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="BKMK_examples"></a>Illustre
Pour réinitialiser le serveur, tapez l’un des éléments suivants :
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande Enable-Server](using-the-enable-server-command.md)
[à l’aide de la commande d’extraction de serveur](using-the-get-server-command.md)
[à l’aide de la commande Initialize-Server](using-the-initialize-server-command.md)
[ Sous-commande : Set-Server](subcommand-set-server.md)1 sous-[commande : Start-Server](subcommand-start-server.md)3 sous-[commande : Stop-Server](subcommand-stop-server.md)
