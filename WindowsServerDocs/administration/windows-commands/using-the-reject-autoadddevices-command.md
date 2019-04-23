---
title: À l’aide de la commande Reject-AutoaddDevices
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af46aec7c8f02b3600983b66bd1b0ac6f5dd1dcc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852560"
---
# <a name="using-the-reject-autoadddevices-command"></a>À l’aide de la commande Reject-AutoaddDevices

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rejette les ordinateurs qui sont en attente d’approbation administrateur. Lorsque la stratégie d’ajout automatique est activée, approbation administrative est exigée avant que les ordinateurs inconnus (ceux qui n’ont pas été prédéfinis) puissent installer une image. Vous pouvez activer cette stratégie à l’aide de la **réponse PXE** onglet de la page de propriétés de serveur s.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
|/RequestId:<Request ID &#124; ALL>|Spécifie l’ID de demande affecté à l’ordinateur en attente. Pour refuser tous les ordinateurs en attente, spécifiez **tous les**.|
## <a name="BKMK_examples"></a>Exemples
Pour rejeter un seul ordinateur, tapez :
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
Pour refuser tous les ordinateurs, tapez :
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande approuver-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[à l’aide de la commande delete-AutoaddDevices](using-the-delete-autoadddevices-command.md) 
 [ À l’aide de la commande get-AutoaddDevices](using-the-get-autoadddevices-command.md)
