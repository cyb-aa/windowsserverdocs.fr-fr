---
title: AllDevices
description: La rubrique commandes Windows pour la commande AllDevices, qui affiche les propriétés des services de déploiement Windows de tous les ordinateurs prédéfinis.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 929a74b6cccaed6e85015648538c1ca875b62208
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831412"
---
# <a name="get-alldevices"></a>AllDevices

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche les propriétés des services de déploiement Windows de tous les ordinateurs prédéfinis. Un ordinateur prédéfini est un ordinateur physique qui a été lié à un compte d’ordinateur dans les services de domaine Active Directory.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Forest : {oui &#124; non}]|Spécifie si les services de déploiement Windows doivent retourner des ordinateurs dans l’ensemble de la forêt ou dans le domaine local. Le paramètre par défaut est **non**, ce qui signifie que seuls les ordinateurs du domaine local sont retournés.|
|[/ReferralServer :<Server name>]|Retourne uniquement les ordinateurs qui sont prédéfinis pour le serveur spécifié.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour afficher tous les ordinateurs, tapez l’un des éléments suivants :
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
sous- [commande : set-Device](subcommand-set-device.md)
[à l’aide de la commande Add-Device](using-the-add-device-command.md)
[à l’aide de la commande](using-the-get-device-command.md) Set-Device
