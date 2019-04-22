---
title: À l’aide de la commande Initialize-Server
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4a1f3a1c02eb21f630aaff7219610864cd1db2a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825190"
---
# <a name="using-the-initialize-server-command"></a>À l’aide de la commande Initialize-Server

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure un serveur de Services de déploiement Windows pour une première utilisation après avoir installé le rôle de serveur. Après avoir exécuté cette commande, vous devez utiliser le [à l’aide de la commande add-Image](using-the-add-image-command.md) commande pour ajouter des images au serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
|/ remInst : «<Full path>»|Spécifie le chemin d’accès complet et le nom du dossier remoteInstall. Si le dossier spécifié n’existe pas déjà, cette option crée lorsque la commande est exécutée. Vous devez toujours entrer un chemin d’accès local, même en cas d’un ordinateur distant. Exemple : **D:\remoteInstall**.|
|[/ Autoriser]|Autorise le serveur de contrôle de protocole DHCP (Dynamic Host). Cette option est nécessaire uniquement si la détection DHCP non autorisés est activée, ce qui signifie que les services PXE Windows Deployment Services serveur doit être autorisé dans DHCP avant que les ordinateurs clients peuvent être traitées. Notez que la détection DHCP non autorisés est désactivée par défaut.|
## <a name="BKMK_examples"></a>Exemples
Pour initialiser le serveur et le dossier partagé remoteInstall sur le lecteur F:, tapez.
```
wdsutil /Initialize-Server /remInst:"F:\remoteInstall"
```
Pour initialiser le serveur et définir le dossier partagé remoteInstall vers le lecteur C:, tapez.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:"C:\remoteInstall"
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande disable-Server](using-the-disable-server-command.md)
[à l’aide de la commande enable-Server](using-the-enable-server-command.md)
[à l’aide du Get-Server commande](using-the-get-server-command.md)
[sous-commande : set-Server](subcommand-set-server.md)
[sous-commande : start-Server](subcommand-start-server.md)
[sous-commande : Stop-Server](subcommand-stop-server.md)
[l’Option de serveur uninitialize](the-uninitialize-server-option.md)
