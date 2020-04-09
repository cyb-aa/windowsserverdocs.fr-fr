---
title: Initialiser-serveur
description: Rubrique relative aux commandes Windows pour Initialize-Server, qui configure un serveur des services de déploiement Windows pour une utilisation initiale après l’installation du rôle de serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c80af07827c889a3dd1c5d3050cd2ca3b4c8f1e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830792"
---
# <a name="initialize-server"></a>Initialiser-serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure un serveur des services de déploiement Windows pour une utilisation initiale après l’installation du rôle de serveur. Après avoir exécuté cette commande, vous devez utiliser la commande de [commande Add-image](using-the-add-image-command.md) pour ajouter des images au serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/remInst :<Full path>|Spécifie le chemin d’accès complet et le nom du dossier remoteInstall. Si le dossier spécifié n’existe pas encore, cette option le crée lors de l’exécution de la commande. Vous devez toujours entrer un chemin d’accès local, même dans le cas d’un ordinateur distant. Par exemple : **D:\remoteInstall**.|
|/Authorize|Autorise le serveur dans le protocole DHCP (Dynamic Host Control Protocol). Cette option est nécessaire uniquement si la détection non fiable DHCP est activée, ce qui signifie que le serveur PXE Windows Deployment Services doit être autorisé dans DHCP avant que les ordinateurs clients puissent être desservis. Notez que la détection non fiable DHCP est désactivée par défaut.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour initialiser le serveur et définir le dossier partagé remoteInstall sur le lecteur F :, tapez.
```
wdsutil /Initialize-Server /remInst:F:\remoteInstall
```
Pour initialiser le serveur et définir le dossier partagé remoteInstall sur le lecteur C :, tapez.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:C:\remoteInstall
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande Enable-Server](using-the-enable-server-command.md)
[à l’aide de la commande](using-the-get-server-command.md) Set-Server
sous-commande [: set-server](subcommand-set-server.md)
sous- [commande : Start-](subcommand-start-server.md) Server
[subcommand : Stop-Server](subcommand-stop-server.md)
[l’option Uninitialize-Server](the-uninitialize-server-option.md)
