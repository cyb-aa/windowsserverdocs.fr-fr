---
title: AllDevices
description: Pour obtenir une rubrique de référence sur la classe AllDevices, qui affiche les propriétés des services de déploiement Windows de tous les ordinateurs prédéfinis.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26e114be7ecf104687da237636b54b79e4114591
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720902"
---
# <a name="get-alldevices"></a>AllDevices

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche les propriétés des services de déploiement Windows de tous les ordinateurs prédéfinis. Un ordinateur prédéfini est un ordinateur physique qui a été lié à un compte d’ordinateur dans les services de domaine Active Directory.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Forest : {Yes &#124; no}]|Spécifie si les services de déploiement Windows doivent retourner des ordinateurs dans l’ensemble de la forêt ou dans le domaine local. Le paramètre par défaut est **non**, ce qui signifie que seuls les ordinateurs du domaine local sont retournés.|
|[/ReferralServer :<Server name>]|Retourne uniquement les ordinateurs qui sont prédéfinis pour le serveur spécifié.|
## <a name="examples"></a>Exemples
Pour afficher tous les ordinateurs, tapez l’un des éléments suivants :
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande sous-[commande : Set-Device](subcommand-set-device.md)
à l'
[aide de la commande Add-Device](using-the-add-device-command.md)[à l’aide de la commande](using-the-get-device-command.md) Set-Device
