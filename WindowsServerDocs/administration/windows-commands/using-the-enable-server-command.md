---
title: Activer-serveur
description: Rubrique relative aux commandes Windows pour Enable-Server, qui active tous les services pour les services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b10ff920667cfdbaae5baaf096bf56e11ce880e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831542"
---
# <a name="enable-server"></a>Activer-serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active tous les services pour les services de déploiement Windows.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour activer les services sur le serveur, exécutez l’une des opérations suivantes :
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande](using-the-get-server-command.md) Set-Server
[à l’aide de la commande Initialize-Server](using-the-initialize-server-command.md)
sous-commande [: set-server](subcommand-set-server.md)
sous- [commande : Start-](subcommand-start-server.md) Server
[subcommand : Stop-Server](subcommand-stop-server.md)
[l’option Uninitialize-Server](the-uninitialize-server-option.md)
