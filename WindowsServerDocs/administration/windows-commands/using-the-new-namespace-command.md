---
title: nouvel espace de noms
description: Rubrique de référence sur New-Namespace, qui crée et configure un nouvel espace de noms.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7bc6b365da274fc62df3bb24375c07b97c8e4bc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82710532"
---
# <a name="new-namespace"></a>nouvel espace de noms

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|Convivial<Friendly name>|Spécifie le nom convivial de l’espace de noms.|
|/Description<Description>]|Définit la description de l’espace de noms.|
|Joint<Namespace name>|Spécifie le nom de l'espace de noms. Notez qu’il ne s’agit pas du nom convivial et qu’il doit être unique.<p>-   **Service de rôle serveur de déploiement**: la syntaxe de cette option est/Namespace :<Image group>/<Image name>/<Index>WDS :. Par exemple : **WDS : ImageGroup1/install. wim/1**<br />-   **Service de rôle du serveur de transport**: cette valeur doit correspondre au nom donné lorsque l’espace de noms a été créé sur le serveur.|
|/ContentProvider :<Name>]|Spécifie le nom du fournisseur de contenu qui fournira le contenu de l’espace de noms.|
|[/ConfigString :<Configuration string>]|Spécifie la chaîne de configuration pour le fournisseur de contenu.|
|/NamespaceType : {AutoCast &#124; ScheduledCast}|Spécifie les paramètres de la transmission. Vous spécifiez les paramètres à l’aide des options suivantes :<p>-[/Time : <time>]-définit l’heure à laquelle la transmission doit commencer en utilisant le format suivant : aaaa/mm/jj : HH : mm. Cette option s’applique uniquement aux transmissions de conversion planifiée.<br />-[/Clients : <Number of clients>]-définit le nombre minimal de clients à attendre avant le démarrage de la transmission. Cette option s’applique uniquement aux transmissions de conversion planifiée.|
## <a name="examples"></a>Exemples
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
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande[à l’aide de la commande](using-the-get-allnamespaces-command.md)
AllNamespaces[à l’aide de la sous-commande Remove-Namespace](using-the-remove-namespace-command.md)
, commande[: Start-namespace](subcommand-start-namespace.md)
