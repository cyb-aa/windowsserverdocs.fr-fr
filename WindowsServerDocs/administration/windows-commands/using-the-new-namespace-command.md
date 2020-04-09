---
title: nouvel espace de noms
description: Rubrique relative aux commandes Windows pour New-Namespace, qui crée et configure un nouvel espace de noms.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f616c33525d033827d52925e07764761e7c7b44
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830632"
---
# <a name="new-namespace"></a>nouvel espace de noms

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée et configure un nouvel espace de noms. Vous devez utiliser cette option lorsque vous n’avez installé que le service de rôle serveur de transport. Si vous avez installé à la fois le service de rôle serveur de déploiement et le service de rôle serveur de transport (valeur par défaut), utilisez [la commande New-MulticastTransmission](using-the-new-multicasttransmission-command.md). Notez que vous devez inscrire le fournisseur de contenu avant d’utiliser cette option.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /New-Namespace [/Server:<Server name>]
     /FriendlyName:<Friendly name>
     [/Description:<Description>]
     /Namespace:<Namespace name>
     /ContentProvider:<Name>
     [/ConfigString:<Configuration string>]
     /Namespacetype: {AutoCast | ScheduledCast}
         [/time:<YYYY/MM/DD:hh:mm>]
         [/Clients:<Number of clients>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/FriendlyName :<Friendly name>|Spécifie le nom convivial de l’espace de noms.|
|/Description<Description>]|Définit la description de l’espace de noms.|
|/Namespace :<Namespace name>|Spécifie le nom de l’espace de noms. Notez qu’il ne s’agit pas du nom convivial et qu’il doit être unique.<p>-   le **service de rôle serveur de déploiement**: la syntaxe de cette option est/Namespace : WDS :<Image group>/<Image name>/<Index>. Par exemple : **WDS : ImageGroup1/install. wim/1**<br />**service de rôle du serveur de Transport**-   : cette valeur doit correspondre au nom donné lorsque l’espace de noms a été créé sur le serveur.|
|/ContentProvider :<Name>]|Spécifie le nom du fournisseur de contenu qui fournira le contenu de l’espace de noms.|
|[/ConfigString :<Configuration string>]|Spécifie la chaîne de configuration pour le fournisseur de contenu.|
|/NamespaceType : {AutoCast &#124; ScheduledCast}|Spécifie les paramètres de la transmission. Vous spécifiez les paramètres à l’aide des options suivantes :<p>-[/Time : <time>]-définit l’heure à laquelle la transmission doit commencer en utilisant le format suivant : AAAA/MM/JJ : HH : mm. Cette option s’applique uniquement aux transmissions de conversion planifiée.<br />-[/Clients : <Number of clients>]-définit le nombre minimal de clients à attendre avant le démarrage de la transmission. Cette option s’applique uniquement aux transmissions de conversion planifiée.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour créer un espace de noms de cast automatique, tapez :
```
wdsutil /New-Namespace /FriendlyName:Custom AutoCast Namespace /Namespace:Custom Auto 1 /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
Pour créer un espace de noms de conversion planifiée, tapez :
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:Custom Scheduled Namespace /Namespace:Custom Auto 1 /ContentProvider:MyContentProvider 
/Namespacetype:ScheduledCast /time:2006/11/20:17:00 /Clients:20
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande AllNamespaces](using-the-get-allnamespaces-command.md)
[à l’aide de la commande Remove-Namespace](using-the-remove-namespace-command.md)
sous- [commande : Start-namespace](subcommand-start-namespace.md)
