---
title: À l’aide de la commande delete-AutoaddDevices
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8b3375418c5ce0b02e187e292cac5b168f0de5dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813500"
---
# <a name="using-the-delete-autoadddevices-command"></a>À l’aide de la commande delete-AutoaddDevices

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime les ordinateurs qui sont en attente, rejeté ou approuvé à partir de la base de données d’ajout automatique. Cette base de données stocke des informations sur ces ordinateurs sur le serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
|/Devicetype:{PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Spécifie le type d’ordinateur à supprimer de la base de données. Il peut s’agir d’un des trois types suivants :<br /><br />-   **PendingDevices** retourne tous les ordinateurs dans la base de données qui ont le statut en attente.<br />-   **RejectedDevices** retourne tous les ordinateurs dans la base de données qu’ont un état rejetés.<br />-   **ApprovedDevices** retourne tous les ordinateurs qui ont le statut approuvé.|
## <a name="BKMK_examples"></a>Exemples
Pour supprimer les ordinateurs tout rejetés, tapez :
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Pour supprimer les ordinateurs approuvés tous les, tapez :
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande approuver-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[à l’aide de la commande get-AutoaddDevices](using-the-get-autoadddevices-command.md) 
 [à l’aide de la commande Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
