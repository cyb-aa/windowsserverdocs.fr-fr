---
title: Sous-commande Set-Device
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 92e319e7c6671e9117e33f13ee8fb40a19df93bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383905"
---
# <a name="subcommand-set-device"></a>Sous-commande : Set-Device

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

modifie les attributs d’un ordinateur prédéfini. Un ordinateur prédéfini est un ordinateur qui a été lié à un objet de compte d’ordinateur dans les serveurs de domaine Active Directory (AD DS). Les clients prédéfinis sont également appelés ordinateurs connus. Vous pouvez configurer les propriétés du compte d’ordinateur pour contrôler l’installation du client. Par exemple, vous pouvez configurer le programme de démarrage réseau et le fichier d’installation sans assistance que le client doit recevoir, ainsi que le serveur à partir duquel le client doit télécharger le programme de démarrage réseau.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] 
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/Device : <computer name>|Spécifie le nom de l’ordinateur (SAM-Account-Name).|
|[/ID : < > &#124; adresse MAC de l’UUID]|Spécifie le GUID/UUID ou l’adresse MAC de l’ordinateur. Cette valeur doit être dans l’un des trois formats suivants :<br /><br />-Chaîne binaire : **/ID : ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-GUID/UUID chaîne:/ID :**E8A3EFAC-201F-4e69-953e-B2DAA1E8B1B6**<br />-Adresse MAC : **00B056882FDC** (sans tirets) ou **00-B0-56-88-2F-DC** (avec des tirets)|
|[/ReferralServer : <Server name>]|Spécifie le nom du serveur à contacter pour télécharger le programme de démarrage réseau et l’image de démarrage à l’aide d’un protocole FTP trivial (TFTP).|
|[/BootProgram : <Relative path>]|Spécifie le chemin d’accès relatif du dossier remoteInstall au programme de démarrage réseau que l’ordinateur spécifié recevra. Par exemple : **boot\x86\pxeboot.com**|
|[/WdsClientUnattend : <Relative path>]|Spécifie le chemin d’accès relatif du dossier remoteInstall au fichier d’installation sans assistance qui automatise les écrans d’installation du client des services de déploiement Windows.|
|[/User : < domaine\utilisateur &#124; User@Domain >]|Définit les autorisations sur l’objet de compte d’ordinateur pour accorder à l’utilisateur spécifié les droits nécessaires pour joindre l’ordinateur au domaine.|
|[/JoinRights : {JoinOnly &#124; complet}]|Spécifie le type de droits à assigner à l’utilisateur.<br /><br />-   **JoinOnly** nécessite que l’administrateur réinitialise le compte d’ordinateur pour que l’utilisateur puisse joindre l’ordinateur au domaine.<br />-   **Full** donne un accès complet à l’utilisateur, y compris le droit de joindre l’ordinateur au domaine.|
|[/JoinDomain : {oui &#124; non}]|Spécifie si l’ordinateur doit être joint au domaine en tant que ce compte d’ordinateur au cours d’une installation des services de déploiement Windows. La valeur par défaut est **Oui**.|
|[/BootImagepath : <Relative path>]|Spécifie le chemin d’accès relatif du dossier remoteInstall à l’image de démarrage que l’ordinateur utilisera.|
|[/Domain : <Domain>]|Spécifie le domaine dans lequel Rechercher l’ordinateur préinstallé. La valeur par défaut est le domaine local.|
|[/resetAccount]|réinitialise les autorisations sur l’ordinateur spécifié afin que quiconque disposant des autorisations appropriées puisse joindre le domaine à l’aide de ce compte.|
## <a name="BKMK_examples"></a>Illustre
Pour définir le programme de démarrage réseau et le serveur de référence pour un ordinateur, tapez :
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
[à l’aide de la commande add-Device](using-the-add-device-command.md)
[à l’aide de la commande AllDevices](using-the-get-alldevices-command.md)
[à l’aide de la commande « main-Device](using-the-get-device-command.md) »
