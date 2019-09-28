---
title: Utilisation de la commande AutoaddDevices
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa30a8ebd73164dc3d8ab267c3deb0739aa4b700
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363247"
---
# <a name="using-the-get-autoadddevices-command"></a>Utilisation de la commande AutoaddDevices

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche tous les ordinateurs qui se trouvent dans la base de données d’ajout automatique sur un serveur des services de déploiement Windows.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/DeviceType : {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|Spécifie le type d’ordinateur à retourner.<br /><br />-   **PendingDevices** retourne tous les ordinateurs de la base de données dont l’État est en attente.<br />-   **RejectedDevices** retourne tous les ordinateurs de la base de données dont l’État est rejeté.<br />-   **ApprovedDevices** retourne tous les ordinateurs de la base de données dont l’État est approuvé.|
## <a name="BKMK_examples"></a>Illustre
Pour afficher tous les ordinateurs approuvés, tapez :
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
Pour afficher tous les ordinateurs rejetés, tapez :
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[à l’aide de la commande approve-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[à l’aide de la commande Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
