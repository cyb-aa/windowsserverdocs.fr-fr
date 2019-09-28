---
title: Utilisation de la commande Enable-Server
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0b90621ec14c6cf451d7a05eace79f2e0679b2f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363453"
---
# <a name="using-the-enable-server-command"></a>Utilisation de la commande Enable-Server

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Active tous les services pour les services de déploiement Windows.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="BKMK_examples"></a>Illustre
Pour activer les services sur le serveur, exécutez l’une des opérations suivantes :
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande-Server](using-the-get-server-command.md)
 à l’aide de[la commande Initialize-Server](using-the-initialize-server-command.md)
 sous-[commande : Set-Server](subcommand-set-server.md)
[ Sous-commande : Start-Server](subcommand-start-server.md)1 sous-[commande : Stop-Server](subcommand-stop-server.md)3[l’option Uninitialize-Server](the-uninitialize-server-option.md)
