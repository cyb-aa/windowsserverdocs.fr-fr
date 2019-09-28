---
title: Utilisation de la commande Initialize-Server
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9e95972838fc70ee1e617d1e299c9e35db5b979
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392095"
---
# <a name="using-the-initialize-server-command"></a>Utilisation de la commande Initialize-Server

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Configure un serveur des services de déploiement Windows pour une utilisation initiale après l’installation du rôle de serveur. Après avoir exécuté cette commande, vous devez utiliser la commande de [commande Add-image](using-the-add-image-command.md) pour ajouter des images au serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/remInst : « <Full path> »|Spécifie le chemin d’accès complet et le nom du dossier remoteInstall. Si le dossier spécifié n’existe pas encore, cette option le crée lors de l’exécution de la commande. Vous devez toujours entrer un chemin d’accès local, même dans le cas d’un ordinateur distant. Exemple : **D:\remoteInstall**.|
|/Authorize|Autorise le serveur dans le protocole DHCP (Dynamic Host Control Protocol). Cette option est nécessaire uniquement si la détection non fiable DHCP est activée, ce qui signifie que le serveur PXE Windows Deployment Services doit être autorisé dans DHCP avant que les ordinateurs clients puissent être desservis. Notez que la détection non fiable DHCP est désactivée par défaut.|
## <a name="BKMK_examples"></a>Illustre
Pour initialiser le serveur et définir le dossier partagé remoteInstall sur le lecteur F :, tapez.
```
wdsutil /Initialize-Server /remInst:"F:\remoteInstall"
```
Pour initialiser le serveur et définir le dossier partagé remoteInstall sur le lecteur C :, tapez.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:"C:\remoteInstall"
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande Enable-Server](using-the-enable-server-command.md)
[à l’aide de la commande-Server](using-the-get-server-command.md)
 sous-commande[: Set-Server](subcommand-set-server.md)
 sous-[commande : Start-Server](subcommand-start-server.md)1 sous-[commande : Stop-Server](subcommand-stop-server.md)3[l’option Uninitialize-Server](the-uninitialize-server-option.md)
