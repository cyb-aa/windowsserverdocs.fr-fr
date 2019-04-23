---
title: À l’aide de la commande get-AllDevices
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5f51bcc2332cced906be1eec3265541ffd2d225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886370"
---
# <a name="using-the-get-alldevices-command"></a>À l’aide de la commande get-AllDevices

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche les propriétés de Services de déploiement Windows de tous les ordinateurs préinstallés. Un ordinateur prédéfini est un ordinateur physique qui a été lié à un compte d’ordinateur dans active directory Domain Services.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/ forêt : {Oui &#124; No}]|Spécifie si les Services de déploiement Windows doit retourner des ordinateurs dans toute la forêt ou le domaine local. Le paramètre par défaut est **non**, ce qui signifie que seuls les ordinateurs dans le domaine local sont retournés.|
|[/ ReferralServer :<Server name>]|Retourne uniquement les ordinateurs qui sont prédéfinis pour le serveur spécifié.|
## <a name="BKMK_examples"></a>Exemples
Pour afficher tous les ordinateurs, tapez une des opérations suivantes :
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[sous-commande : set-Device](subcommand-set-device.md)
[à l’aide de la commande add-appareil](using-the-add-device-command.md)
[à l’aide de l’appareil de get Commande](using-the-get-device-command.md)
