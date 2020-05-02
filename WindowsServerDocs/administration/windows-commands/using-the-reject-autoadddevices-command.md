---
title: Reject-AutoaddDevices
description: Rubrique de référence pour Reject-AutoaddDevices, qui rejette les ordinateurs qui sont en attente d’approbation administrative.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e377d4e2d4aecea2e0ba3af023af39ab7695c0a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725925"
---
# <a name="reject-autoadddevices"></a>Reject-AutoaddDevices

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rejette les ordinateurs qui sont en attente d’approbation administrative. Lorsque la stratégie d’ajout automatique est activée, l’approbation administrative est nécessaire avant que les ordinateurs inconnus (ceux qui ne sont pas prédéfinis) puissent installer une image. Vous pouvez activer cette stratégie à l’aide de l’onglet **réponse PXE** de la page Propriétés du serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/RequestId : ID de demande <&#124; tous les>|Spécifie l’ID de demande affecté à l’ordinateur en attente. Pour rejeter tous les ordinateurs en attente, spécifiez **All**.|
## <a name="examples"></a>Exemples
Pour rejeter un seul ordinateur, tapez :
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
Pour refuser tous les ordinateurs, tapez :
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande[à l’aide de la commande Approve-AutoaddDevices](using-the-approve-autoadddevices-command.md)
à l’aide de la commande[Delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[à l’aide de la commande AutoaddDevices](using-the-get-autoadddevices-command.md)
