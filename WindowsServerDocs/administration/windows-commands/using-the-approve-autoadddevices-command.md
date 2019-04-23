---
title: À l’aide de la commande AutoaddDevices approuver
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ce9824c45a00ccb9f1f9e357c7e3d36b2857f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886820"
---
# <a name="using-the-approve-autoadddevices-command"></a>À l’aide de la commande AutoaddDevices approuver

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Approuve les ordinateurs qui sont en attente d’approbation administrateur. Lorsque la stratégie d’ajout automatique est activée, approbation administrative est exigée avant que les ordinateurs inconnus (ceux qui n’ont pas été prédéfinis) puissent installer une image. Vous pouvez activer cette stratégie à l’aide de la **réponse PXE** onglet de la page de propriétés de serveur s.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n'est spécifié, le serveur local est utilisé.|
|/RequestId:{Request ID &#124; ALL}|Spécifie l’ID de demande affecté à l’ordinateur en attente. Spécifiez **tous les** à approuver tous les ordinateurs en attente.|
|[/ MachineName :<Device name>]|Spécifie le nom de l’ordinateur à ajouter. Vous ne pouvez pas utiliser cette option lors de l’approbation de tous les ordinateurs.|
|[/ UNITÉ D’ORGANISATION :<DN of OU>]|Spécifie le nom unique de l’unité d’organisation (UO) où l’objet de compte d’ordinateur doit être créé. Exemple : **Unité d’organisation = MyOU, CN = Test, DC = Domain, DC = com**. L’emplacement par défaut est le conteneur de l’ordinateur par défaut.|
|[/ Utilisateur : < domaine\utilisateur &#124; User@Domain>]|Définit des autorisations sur l’objet de compte d’ordinateur pour attribuer des droits nécessaires de l’utilisateur spécifié.|
|[/ JoinRights : {JoinOnly &#124; complète}]|Spécifie le type de droits à assigner à l’utilisateur spécifié.<br /><br />-   **JoinOnly** nécessite que l’administrateur réinitialiser le compte d’ordinateur avant que l’utilisateur puisse joindre l’ordinateur au domaine.<br />-   **Complète** donne un accès complet à l’utilisateur, ce qui inclut le droit de joindre l’ordinateur au domaine.|
|[/ JoinDomain : {Oui &#124; No}]|Spécifie si l’ordinateur doit être joint au domaine en tant que ce compte d’ordinateur pendant l’installation du système d’exploitation. La valeur par défaut est **Oui**.|
|[/ ReferralServer :<Server name>]|Spécifie le nom du serveur d’être contacté pour télécharger l’image de démarrage et le programme de démarrage réseau en utilisant le protocole Trivial FTP (tftp).|
|[/ BootProgram :<Relative path>]|Spécifie le chemin d’accès relatif à partir du dossier remoteInstall du programme de démarrage réseau auxquels cet ordinateur. Par exemple : **boot\x86\pxeboot.com**.|
|[/WdsClientUnattend:<Relative path>]|Spécifie le chemin d’accès relatif à partir du dossier remoteInstall dans le fichier d’installation sans assistance qui automatise le client des Services de déploiement Windows.|
|[/ BootImagepath :<Relative path>]|Spécifie le chemin d’accès relatif à partir du dossier remoteInstall à l’image de démarrage que cet ordinateur doit recevoir.|
## <a name="BKMK_examples"></a>Exemples
Pour approuver l’ordinateur avec un ID de demande de 12, tapez :
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
Pour approuver l’ordinateur avec un ID de demande de 20 et déployer l’image avec les paramètres spécifiés, tapez :
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:"OU=Test,CN=company,DC=Domain,DC=Com" /User:Domain\User1 
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
Pour approuver tous les ordinateurs en attente, tapez :
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[à l’aide de la commande get-AutoaddDevices](using-the-get-autoadddevices-command.md) 
 [à l’aide de la commande Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
