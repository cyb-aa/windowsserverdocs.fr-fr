---
title: Activer-serveur
description: Rubrique de référence pour Enable-Server, qui active tous les services pour les services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf7bf57c0784fa16719b9f77da50212bca0ef850
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720934"
---
# <a name="enable-server"></a>Activer-serveur

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active tous les services pour les services de déploiement Windows.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
## <a name="examples"></a>Exemples
Pour activer les services sur le serveur, exécutez l’une des opérations suivantes :
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande à l'
[aide de la commande Disable-Server](using-the-disable-server-command.md)à l’aide[de la commande](using-the-get-server-command.md)
Set-Server à l’aide de la sous-commande de[commande](using-the-initialize-server-command.md)
Initialize-Server :[Set-Server](subcommand-set-server.md)
, sous-commande[: Start-Server](subcommand-start-server.md)
, sous[-commande : Stop-Server](subcommand-stop-server.md)
[l’option Uninitialize-Server](the-uninitialize-server-option.md)
