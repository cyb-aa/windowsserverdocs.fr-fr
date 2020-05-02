---
title: supprimer-AutoaddDevices
description: Rubrique de référence pour Delete-AutoaddDevices, qui supprime les ordinateurs en attente, rejetés ou approuvés par la base de données d’ajout automatique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 90b5b24b68b2cfe3d387cb02b3715b70edba4300
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720995"
---
# <a name="delete-autoadddevices"></a>supprimer-AutoaddDevices

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime les ordinateurs en attente, rejetés ou approuvés à partir de la base de données d’ajout automatique. Cette base de données stocke des informations sur ces ordinateurs sur le serveur.

## <a name="syntax"></a>Syntaxe
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/DeviceType : {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Spécifie le type d’ordinateur à supprimer de la base de données. Il peut s’agir de l’un des trois types suivants :<p>-   **PendingDevices** retourne tous les ordinateurs de la base de données dont l’État est en attente.<br />-   **RejectedDevices** retourne tous les ordinateurs de la base de données dont l’État est rejeté.<br />-   **ApprovedDevices** retourne tous les ordinateurs dont l’État est approuvé.|
## <a name="examples"></a>Exemples
Pour supprimer tous les ordinateurs rejetés, tapez :
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Pour supprimer tous les ordinateurs approuvés, tapez :
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande[à l’aide de la commande Approve-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[à](using-the-get-autoadddevices-command.md)
l’aide de la commande-AutoaddDevices[à l’aide de la commande Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
