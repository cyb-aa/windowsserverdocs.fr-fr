---
title: Approuver-AutoaddDevices
description: Rubrique relative aux commandes Windows pour approuver-AutoaddDevices, qui approuve les ordinateurs qui sont en attente d’approbation administrative.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7f63faabf37337136cad186fd252915adf1a743
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831832"
---
# <a name="approve-autoadddevices"></a>Approuver-AutoaddDevices

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Approuve les ordinateurs qui sont en attente d’approbation administrative. Lorsque la stratégie d’ajout automatique est activée, l’approbation administrative est nécessaire avant que les ordinateurs inconnus (ceux qui ne sont pas prédéfinis) puissent installer une image. Vous pouvez activer cette stratégie à l’aide de l’onglet **réponse PXE** de la page Propriétés du serveur.

## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n'est spécifié, le serveur local est utilisé.|
|/RequestId : {ID &#124; de demande tout}|Spécifie l’ID de demande affecté à l’ordinateur en attente. Spécifiez **All** pour approuver tous les ordinateurs en attente.|
|[/MachineName :<Device name>]|Spécifie le nom de l’ordinateur à ajouter. Vous ne pouvez pas utiliser cette option lors de l’approbation de tous les ordinateurs.|
|[/OU :<DN of OU>]|Spécifie le nom unique de l’unité d’organisation dans laquelle l’objet compte d’ordinateur doit être créé. Par exemple : **ou = MyOU, CN = test, DC = domain, DC = com**. L’emplacement par défaut est le conteneur de l’ordinateur par défaut.|
|[/User : < domaine\utilisateur &#124; User@Domain>]|Définit les autorisations sur l’objet de compte d’ordinateur pour affecter l’utilisateur spécifié aux droits nécessaires.|
|[/JoinRights : {JoinOnly &#124; complet}]|Spécifie le type de droits à assigner à l’utilisateur spécifié.<p>-   **JoinOnly** nécessite que l’administrateur réinitialise le compte d’ordinateur pour que l’utilisateur puisse joindre l’ordinateur au domaine.<br />-   **Full** donne un accès complet à l’utilisateur, ce qui inclut le droit de joindre l’ordinateur au domaine.|
|[/JoinDomain : {oui &#124; non}]|Spécifie si l’ordinateur doit être joint au domaine en tant que ce compte d’ordinateur lors de l’installation du système d’exploitation. La valeur par défaut est **Oui**.|
|[/ReferralServer :<Server name>]|Spécifie le nom du serveur à contacter pour télécharger le programme de démarrage réseau et l’image de démarrage à l’aide d’une protocole FTP simple (TFTP).|
|[/BootProgram :<Relative path>]|Spécifie le chemin d’accès relatif du dossier remoteInstall au programme de démarrage réseau que cet ordinateur doit recevoir. Par exemple : **boot\x86\pxeboot.com**.|
|[/WdsClientUnattend :<Relative path>]|Spécifie le chemin d’accès relatif du dossier remoteInstall au fichier d’installation sans assistance qui automatise le client des services de déploiement Windows.|
|[/BootImagepath :<Relative path>]|Spécifie le chemin d’accès relatif du dossier remoteInstall à l’image de démarrage que cet ordinateur doit recevoir.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour approuver l’ordinateur avec un RequestId de 12, tapez :
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
Pour approuver l’ordinateur avec un RequestID de 20 et déployer l’image avec les paramètres spécifiés, tapez :
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:OU=Test,CN=company,DC=Domain,DC=Com /User:Domain\User1 
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
Pour approuver tous les ordinateurs en attente, tapez :
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[à l’aide de la commande AutoaddDevices](using-the-get-autoadddevices-command.md)
à l' [aide de la commande Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
