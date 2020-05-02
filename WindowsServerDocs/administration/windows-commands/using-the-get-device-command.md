---
title: récupération de l’appareil
description: Rubrique de référence pour l’installation de l’appareil, qui récupère des informations sur les services de déploiement Windows concernant un ordinateur prédéfini (c’est-à-dire, un ordinateur physique qui a été aligné sur un compte d’ordinateur dans les services de domaine Active Directory.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f89266a2f70523ec332ed7cfb6a976f87a8e4f2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719956"
---
# <a name="get-device"></a>récupération de l’appareil

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Récupère des informations sur les services de déploiement Windows concernant un ordinateur prédéfini (c’est-à-dire, un ordinateur physique qui a été aligné sur un compte d’ordinateur dans les services de domaine Active Directory.

## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|Passerelle<Device name>|Spécifie le nom de l’ordinateur (SAMAccountName).|
|Identifi<MAC or UUID>|Spécifie l’adresse MAC ou l’UUID (GUID) de l’ordinateur, comme indiqué dans les exemples suivants. Notez qu’un GUID valide doit être dans l’un des deux formats chaîne binaire ou chaîne GUID<p>-   **Chaîne binaire**:/ID : ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **Adresse Mac**: 00B056882FDC (sans tirets) ou 00-B0-56-88-2F-DC (avec des tirets)<br />-   **Chaîne GUID**:/ID : E8A3EFAC-201F-4e69-953-B2DAA1E8B1B6|
|[/Domain :<Domain>]|Spécifie le domaine dans lequel Rechercher l’ordinateur préinstallé. La valeur par défaut de ce paramètre est le domaine local.|
|[/Forest : {Yes &#124; no}]|Spécifie si les services de déploiement Windows doivent effectuer une recherche dans l’ensemble de la forêt ou dans le domaine local. La valeur par défaut est **non**, ce qui signifie que seul le domaine local sera recherché.|
## <a name="examples"></a>Exemples
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
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande sous-[commande : Set-Device](subcommand-set-device.md)
à l'
[aide de la commande Add-Device](using-the-add-device-command.md)[à l’aide de la commande AllDevices](using-the-get-alldevices-command.md)
