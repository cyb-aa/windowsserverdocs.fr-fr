---
title: Utilisation de la commande « main-Device »
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6451e0a55a72fc88867a3f3be0e1317d881391aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363196"
---
# <a name="using-the-get-device-command"></a>Utilisation de la commande « main-Device »

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Récupère des informations sur les services de déploiement Windows concernant un ordinateur prédéfini (c’est-à-dire, un ordinateur physique qui a été aligné sur un compte d’ordinateur dans les services de domaine Active Directory.
## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/Device : <Device name>|Spécifie le nom de l’ordinateur (SAMAccountName).|
|/ID : <MAC or UUID>|Spécifie l’adresse MAC ou l’UUID (GUID) de l’ordinateur, comme indiqué dans les exemples suivants. Notez qu’un GUID valide doit être dans l’un des deux formats chaîne binaire ou chaîne GUID<br /><br />-   **chaîne binaire**:/ID : ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **adresse Mac**: 00B056882FDC (sans tirets) ou 00-B0-56-88-2F-DC (avec des tirets)<br />**chaîne GUID**-   :/ID : E8A3EFAC-201F-4e69-953-B2DAA1E8B1B6|
|[/Domain : <Domain>]|Spécifie le domaine dans lequel Rechercher l’ordinateur préinstallé. La valeur par défaut de ce paramètre est le domaine local.|
|[/Forest : {oui &#124; non}]|Spécifie si les services de déploiement Windows doivent effectuer une recherche dans l’ensemble de la forêt ou dans le domaine local. La valeur par défaut est **non**, ce qui signifie que seul le domaine local sera recherché.|
## <a name="BKMK_examples"></a>Illustre
Pour récupérer des informations à l’aide du nom de l’ordinateur, tapez :
```
wdsutil /Get-Device /Device:computer1
```
Pour récupérer des informations à l’aide de l’adresse MAC, tapez :
```
wdsutil /verbose /Get-Device /ID:"00-B0-56-88-2F-DC" /Domain:MyDomain
```
Pour récupérer des informations à l’aide de la chaîne GUID, tapez :
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
 sous-[commande : set-Device](subcommand-set-device.md)
[à l’aide de la commande Add-Device](using-the-add-device-command.md)
[à l’aide de la commande AllDevices](using-the-get-alldevices-command.md)
