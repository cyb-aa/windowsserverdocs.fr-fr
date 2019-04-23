---
title: Sous-commande set-Device
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 401567f8-eaeb-4a2d-b811-140bb007028d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80bb9144936cf493784603bcbdb8a0d1e5c870bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848060"
---
# <a name="subcommand-set-device"></a>Sous-commande : set-Device

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie les attributs d’un ordinateur prédéfini. Un ordinateur prédéfini est un ordinateur qui a été lié à un objet de compte d’ordinateur dans les serveurs de domaine active directory (AD DS). Aux clients prédéfinis sont également appelés ordinateurs connus. Vous pouvez configurer des propriétés sur le compte d’ordinateur pour contrôler l’installation du client. Par exemple, vous pouvez configurer le programme de démarrage réseau et le fichier d’installation sans assistance que le client reçoit, ainsi que le serveur à partir duquel le client doit télécharger le programme de démarrage réseau.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] 
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/ APPAREIL :<computer name>|Spécifie le nom de l’ordinateur (nom de compte SAM).|
|[/ ID : < UUID &#124; adresse MAC >]|Spécifie le GUID/UUID ou l’adresse MAC de l’ordinateur. Cette valeur doit être dans un des formats suivants :<br /><br />-Chaîne binaire : **ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-Chaîne GUID/UUID : / ID :**E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br />-Adresse MAC : **00B056882FDC** (sans tirets) ou **00-B0-56-88-2F-DC** (avec des tirets)|
|[/ ReferralServer :<Server name>]|Spécifie le nom du serveur d’être contacté pour télécharger l’image de démarrage et le programme de démarrage réseau en utilisant le protocole Trivial FTP (tftp).|
|[/ BootProgram :<Relative path>]|Spécifie le chemin d’accès relatif à partir du dossier remoteInstall du programme de démarrage réseau qui reçoit l’ordinateur spécifié. Par exemple : **boot\x86\pxeboot.com**|
|[/WdsClientUnattend:<Relative path>]|Spécifie le chemin d’accès relatif à partir du dossier remoteInstall dans le fichier d’installation sans assistance qui automatise les écrans d’installation pour le client des Services de déploiement Windows.|
|[/ Utilisateur : < domaine\utilisateur &#124; User@Domain>]|Définit des autorisations sur l’objet de compte d’ordinateur afin de donner des droits nécessaires pour joindre l’ordinateur au domaine de l’utilisateur spécifié.|
|[/ JoinRights : {JoinOnly &#124; complète}]|Spécifie le type de droits à assigner à l’utilisateur.<br /><br />-   **JoinOnly** nécessite que l’administrateur réinitialiser le compte d’ordinateur avant que l’utilisateur puisse joindre l’ordinateur au domaine.<br />-   **Complète** donne un accès complet à l’utilisateur, y compris le droit de joindre l’ordinateur au domaine.|
|[/ JoinDomain : {Oui &#124; No}]|Spécifie si l’ordinateur doit être joint au domaine en tant que ce compte d’ordinateur lors d’une installation de Services de déploiement Windows. Le paramètre par défaut est **Oui**.|
|[/ BootImagepath :<Relative path>]|Spécifie le chemin d’accès relatif à partir du dossier remoteInstall à l’image de démarrage qui utilise l’ordinateur.|
|[/ Domaine :<Domain>]|Spécifie le domaine à rechercher l’ordinateur prédéfini. La valeur par défaut est le domaine local.|
|[/resetAccount]|Réinitialise les autorisations sur l’ordinateur spécifié afin que toute personne disposant des autorisations appropriées puisse joindre le domaine à l’aide de ce compte.|
## <a name="BKMK_examples"></a>Exemples
Pour définir le serveur de programme et de redirection de démarrage réseau pour un ordinateur, tapez :
```
wdsutil /Set-Device /Device:computer1 /ReferralServer:MyWDSServer
/BootProgram:boot\x86\pxeboot.n12
```
Pour définir différents paramètres pour un ordinateur, tapez :
```
wdsutil /verbose /Set-Device /Device:computer2 /ID:00-B0-56-88-2F-DC /WdsClientUnattend:WDSClientUnattend\unattend.xml 
/User:Domain\user /JoinRights:JoinOnly /JoinDomain:No /BootImagepath:boot\x86\images\boot.wim /Domain:NorthAmerica /resetAccount
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-appareil](using-the-add-device-command.md)
[à l’aide de la commande get-AllDevices](using-the-get-alldevices-command.md)
[à l’aide du Commande de Get-appareil](using-the-get-device-command.md)
