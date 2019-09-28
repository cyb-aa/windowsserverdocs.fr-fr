---
title: Utilisation de la commande AllDevices
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5b7d2ce709c7e3fbaf7ab4f0e49be14c98ba1cd9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401975"
---
# <a name="using-the-get-alldevices-command"></a>Utilisation de la commande AllDevices

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche les propriétés des services de déploiement Windows de tous les ordinateurs prédéfinis. Un ordinateur prédéfini est un ordinateur physique qui a été lié à un compte d’ordinateur dans les services de domaine Active Directory.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Forest : {oui &#124; non}]|Spécifie si les services de déploiement Windows doivent retourner des ordinateurs dans l’ensemble de la forêt ou dans le domaine local. Le paramètre par défaut est **non**, ce qui signifie que seuls les ordinateurs du domaine local sont retournés.|
|[/ReferralServer : <Server name>]|Retourne uniquement les ordinateurs qui sont prédéfinis pour le serveur spécifié.|
## <a name="BKMK_examples"></a>Illustre
Pour afficher tous les ordinateurs, tapez l’un des éléments suivants :
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
 sous-[commande : set-Device](subcommand-set-device.md)
[à l’aide de la commande Add-Device](using-the-add-device-command.md)
[à l’aide de la commande Set-Device](using-the-get-device-command.md)
