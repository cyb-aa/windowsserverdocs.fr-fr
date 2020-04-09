---
title: récupération de l’appareil
description: La rubrique commandes Windows pour l’installation de l’appareil, qui récupère des informations sur les services de déploiement Windows concernant un ordinateur prédéfini (c’est-à-dire, un ordinateur physique qui a été aligné sur un compte d’ordinateur dans les services de domaine Active Directory).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b9554ed6236d02be0be3502f42552bafbbfe1cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831112"
---
# <a name="get-device"></a>récupération de l’appareil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère des informations sur les services de déploiement Windows concernant un ordinateur prédéfini (c’est-à-dire, un ordinateur physique qui a été aligné sur un compte d’ordinateur dans les services de domaine Active Directory.

## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/Device :<Device name>|Spécifie le nom de l’ordinateur (SAMAccountName).|
|/ID :<MAC or UUID>|Spécifie l’adresse MAC ou l’UUID (GUID) de l’ordinateur, comme indiqué dans les exemples suivants. Notez qu’un GUID valide doit être dans l’un des deux formats chaîne binaire ou chaîne GUID<p>-   **chaîne binaire**:/ID : ACEFA3E81F20694E953EB2DAA1E8B1B6<br />**adresse MAC**-   : 00B056882FDC (sans tirets) ou 00-B0-56-88-2F-DC (avec des tirets)<br />-   **chaîne GUID**:/ID : E8A3EFAC-201F-4e69-953-B2DAA1E8B1B6|
|[/Domain :<Domain>]|Spécifie le domaine dans lequel Rechercher l’ordinateur préinstallé. La valeur par défaut de ce paramètre est le domaine local.|
|[/Forest : {oui &#124; non}]|Spécifie si les services de déploiement Windows doivent effectuer une recherche dans l’ensemble de la forêt ou dans le domaine local. La valeur par défaut est **non**, ce qui signifie que seul le domaine local sera recherché.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour récupérer des informations à l’aide du nom de l’ordinateur, tapez :
```
wdsutil /Get-Device /Device:computer1
```
Pour récupérer des informations à l’aide de l’adresse MAC, tapez :
```
wdsutil /verbose /Get-Device /ID:00-B0-56-88-2F-DC /Domain:MyDomain
```
Pour récupérer des informations à l’aide de la chaîne GUID, tapez :
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
sous- [commande : set-Device](subcommand-set-device.md)
[à l’aide de la commande Add-Device](using-the-add-device-command.md)
[à l’aide de la commande AllDevices](using-the-get-alldevices-command.md)
