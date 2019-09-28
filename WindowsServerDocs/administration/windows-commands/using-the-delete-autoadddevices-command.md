---
title: Utilisation de la commande Delete-AutoaddDevices
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e914506f731685b17117b359359f60ea91992dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363536"
---
# <a name="using-the-delete-autoadddevices-command"></a>Utilisation de la commande Delete-AutoaddDevices

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

supprime les ordinateurs en attente, rejetés ou approuvés à partir de la base de données d’ajout automatique. Cette base de données stocke des informations sur ces ordinateurs sur le serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/DeviceType : {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Spécifie le type d’ordinateur à supprimer de la base de données. Il peut s’agir de l’un des trois types suivants :<br /><br />-   **PendingDevices** retourne tous les ordinateurs de la base de données dont l’État est en attente.<br />-   **RejectedDevices** retourne tous les ordinateurs de la base de données dont l’État est rejeté.<br />-   **ApprovedDevices** retourne tous les ordinateurs dont l’État est approuvé.|
## <a name="BKMK_examples"></a>Illustre
Pour supprimer tous les ordinateurs rejetés, tapez :
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Pour supprimer tous les ordinateurs approuvés, tapez :
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande Approve-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[à l’aide de la commande AutoaddDevices](using-the-get-autoadddevices-command.md)
[à l’aide de la commande Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
