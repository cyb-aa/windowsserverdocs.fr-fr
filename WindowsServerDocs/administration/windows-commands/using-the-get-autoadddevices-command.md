---
title: À l’aide de la commande get-AutoaddDevices
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 337c8e76923fe243982ba9c10d18f2e5a5e7d9ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885740"
---
# <a name="using-the-get-autoadddevices-command"></a>À l’aide de la commande get-AutoaddDevices

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche tous les ordinateurs qui se trouvent dans la base de données d’ajout automatique sur un serveur de Services de déploiement Windows.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
|/ Devicetype : {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|Spécifie le type d’ordinateur à retourner.<br /><br />-   **PendingDevices** retourne tous les ordinateurs dans la base de données qui ont le statut en attente.<br />-   **RejectedDevices** retourne tous les ordinateurs dans la base de données qu’ont un état rejetés.<br />-   **ApprovedDevices** retourne tous les ordinateurs dans la base de données qui ont un état approuvé.|
## <a name="BKMK_examples"></a>Exemples
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
[à l’aide de la commande approuver-AutoaddDevices](using-the-approve-autoadddevices-command.md) 
 [ À l’aide de la commande Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
