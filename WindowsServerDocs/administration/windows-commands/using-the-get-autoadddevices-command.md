---
title: AutoaddDevices
description: Pour obtenir une rubrique de référence sur la AutoaddDevices, qui affiche tous les ordinateurs qui se trouvent dans la base de données d’ajout automatique sur un serveur des services de déploiement Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c15836fa81c694aa9295d0a98376f4bef3125243
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719986"
---
# <a name="get-autoadddevices"></a>AutoaddDevices

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche tous les ordinateurs qui se trouvent dans la base de données d’ajout automatique sur un serveur des services de déploiement Windows.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/DeviceType : {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|Spécifie le type d’ordinateur à retourner.<p>-   **PendingDevices** retourne tous les ordinateurs de la base de données dont l’État est en attente.<br />-   **RejectedDevices** retourne tous les ordinateurs de la base de données dont l’État est rejeté.<br />-   **ApprovedDevices** retourne tous les ordinateurs de la base de données dont l’État est approuvé.|
## <a name="examples"></a>Exemples
Pour afficher tous les ordinateurs approuvés, tapez :
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
Pour afficher tous les ordinateurs rejetés, tapez :
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande[à l’aide de la commande Delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
à l’aide de la commande[Approve-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[à l’aide de la commande Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
