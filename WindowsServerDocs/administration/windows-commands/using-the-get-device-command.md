---
title: À l’aide de la commande get-appareil
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 117720c5102da5713c1b0bb5e59cbcc099f3aa8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871030"
---
# <a name="using-the-get-device-command"></a>À l’aide de la commande get-appareil

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère des informations de Services de déploiement Windows sur un ordinateur prédéfini (autrement dit, un ordinateur physique qui a été inline avec un compte d’ordinateur dans active directory Domain Services.
## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/ APPAREIL :<Device name>|Spécifie le nom de l’ordinateur (SAMAccountName).|
|/ ID :<MAC or UUID>|Spécifie l’adresse MAC ou l’UUID (GUID) de l’ordinateur, comme indiqué dans les exemples suivants. Notez qu’un GUID valide doit être dans un des deux formats de chaîne binaire ou chaîne GUID<br /><br />-   **Chaîne binaire**: ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **Adresse MAC**: 00B056882FDC (sans tirets) ou 00-B0-56-88-2F-DC (avec des tirets)<br />-   **Chaîne GUID**: E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/ Domaine :<Domain>]|Spécifie le domaine à rechercher l’ordinateur prédéfini. La valeur par défaut pour ce paramètre est le domaine local.|
|[/ forêt : {Oui &#124; No}]|Spécifie si les Services de déploiement Windows doit rechercher l’ensemble de la forêt ou le domaine local. La valeur par défaut est **non**, ce qui signifie que seul le domaine local sera recherché.|
## <a name="BKMK_examples"></a>Exemples
Pour obtenir des informations en utilisant le nom d’ordinateur, tapez :
```
wdsutil /Get-Device /Device:computer1
```
Pour obtenir des informations à l’aide de l’adresse MAC, tapez :
```
wdsutil /verbose /Get-Device /ID:"00-B0-56-88-2F-DC" /Domain:MyDomain
```
Pour obtenir des informations à l’aide de la chaîne GUID, tapez :
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[sous-commande : set-Device](subcommand-set-device.md)
[à l’aide de la commande add-appareil](using-the-add-device-command.md)
[à l’aide du Commande de Get-AllDevices](using-the-get-alldevices-command.md)
