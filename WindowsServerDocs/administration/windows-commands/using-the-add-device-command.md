---
title: À l’aide de la commande add-appareil
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85e9ef4445b4dabbe85c2397d62b06756e17879d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878540"
---
# <a name="using-the-add-device-command"></a>À l’aide de la commande add-appareil

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Prépare un ordinateur dans active directory Domain Services. Ordinateurs préinstallés sont également appelés ordinateurs connus. Cela vous permet de configurer les propriétés pour contrôler l’installation du client. Par exemple, vous pouvez configurer le programme de démarrage réseau et le fichier d’installation sans assistance que le client reçoit, ainsi que le serveur à partir duquel le client doit télécharger le programme de démarrage réseau.
Pour obtenir des exemples de la façon dont vous pouvez utiliser cette commande, consultez [exemples](#BKMK_examples).
## <a name="syntax"></a>Syntaxe
```
wdsutil /add-Device /Device:<Device name> /ID:<UUID | MAC address> [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/OU:<DN of OU>] [/Domain:<Domain>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/ APPAREIL :<computer name>|Spécifie le nom de l’ordinateur à ajouter.|
|/ ID : < UUID &#124; adresse MAC >|Spécifie le GUID/UUID ou l’adresse MAC de l’ordinateur. Un GUID/UUID doit être l’un de chaîne GUID ou de chaîne binaire de deux formats. Exemple :<br /><br />Chaîne binaire : **ACEFA3E81F20694E953EB2DAA1E8B1B6**<br /><br />Chaîne GUID : **E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br /><br />Une adresse MAC doit être au format suivant : **00B056882FDC** (sans tirets) ou **00-B0-56-88-2F-DC** (avec des tirets)|
|[/ ReferralServer :<Server name>]|Spécifie le nom du serveur d’être contacté pour télécharger le programme de démarrage réseau et l’image de démarrage en utilisant le protocole Trivial FTP (tftp).|
|[/ BootProgram :<Relative path>]|Spécifie le chemin d’accès relatif à partir du dossier remoteInstall du programme de démarrage réseau auxquels cet ordinateur. Par exemple : « boot\x86\pxeboot.com »|
|[/WdsClientUnattend:<Relative path>]|Spécifie le chemin d’accès relatif à partir du dossier remoteInstall pour le fichier d’installation sans assistance qui automatise les écrans d’installation du client des Services de déploiement Windows.|
|[/ Utilisateur : < domaine\utilisateur &#124; User@Domain>]|Définit des autorisations sur l’objet de compte d’ordinateur afin de donner des droits nécessaires pour joindre l’ordinateur au domaine de l’utilisateur spécifié.|
|[/ JoinRights : {JoinOnly &#124; complète}]|Spécifie le type de droits à assigner à l’utilisateur.<br /><br />-   **JoinOnly** nécessite que l’administrateur réinitialiser le compte d’ordinateur avant que l’utilisateur puisse joindre l’ordinateur au domaine.<br />-   **Complète** donne un accès complet à l’utilisateur, ce qui inclut le droit de joindre l’ordinateur au domaine.|
|[/ JoinDomain : {Oui &#124; No}]|Spécifie si l’ordinateur doit être joint au domaine en tant que ce compte d’ordinateur pendant l’installation du système d’exploitation. La valeur par défaut est **Oui**.|
|[/ BootImagepath :<Relative path>]|Spécifie le chemin d’accès relatif à partir du dossier remoteInstall à l’image de démarrage que cet ordinateur doit utiliser.|
|[/ UNITÉ D’ORGANISATION :<DN of OU>]|Le nom unique de l’unité d’organisation où l’objet de compte d’ordinateur doit être créé. Exemple : **Unité d’organisation = MyOU, CN = Test, DC = Domain, DC = com**. L’emplacement par défaut est le conteneur de l’ordinateur par défaut.|
|[/ Domaine :<Domain>]|Le domaine où l’objet de compte d’ordinateur doit être créé. L’emplacement par défaut est le domaine local.|
## <a name="BKMK_examples"></a>Exemples
Pour ajouter un ordinateur à l’aide d’une adresse MAC, tapez :
```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```
Pour ajouter un ordinateur à l’aide d’une chaîne GUID, tapez :
```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com 
/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:"OU=MyOU,CN=Test,DC=Domain,DC=com"
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande get-AllDevices](using-the-get-alldevices-command.md)
[à l’aide de la commande get-appareil](using-the-get-device-command.md)
[sous-commande : Set-Device](subcommand-set-device.md)
[New-WdsClient](https://technet.microsoft.com/library/dn283430.aspx)
