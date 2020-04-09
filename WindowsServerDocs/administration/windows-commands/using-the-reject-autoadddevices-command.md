---
title: Reject-AutoaddDevices
description: La rubrique commandes Windows pour Reject-AutoaddDevices, qui rejette les ordinateurs qui sont en attente d’approbation administrative.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39bdddbbc169a0a0810fcc67e95224360858b728
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830652"
---
# <a name="reject-autoadddevices"></a>Reject-AutoaddDevices

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rejette les ordinateurs qui sont en attente d’approbation administrative. Lorsque la stratégie d’ajout automatique est activée, l’approbation administrative est nécessaire avant que les ordinateurs inconnus (ceux qui ne sont pas prédéfinis) puissent installer une image. Vous pouvez activer cette stratégie à l’aide de l’onglet **réponse PXE** de la page Propriétés du serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/RequestId : < > l' &#124; ID de la demande|Spécifie l’ID de demande affecté à l’ordinateur en attente. Pour rejeter tous les ordinateurs en attente, spécifiez **All**.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour rejeter un seul ordinateur, tapez :
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
Pour refuser tous les ordinateurs, tapez :
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande Approve-AutoaddDevices](using-the-approve-autoadddevices-command.md)
à l’aide de la commande [Delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[à l’aide de la commande AutoaddDevices](using-the-get-autoadddevices-command.md)
