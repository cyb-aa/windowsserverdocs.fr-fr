---
title: À l’aide de la commande Nouveau Namespace
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50d51101afe95c99b7034fc50b3d30b799ee02ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871150"
---
# <a name="using-the-new-namespace-command"></a>À l’aide de la commande Nouveau Namespace

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée et configure un nouvel espace de noms. Vous devez utiliser cette option lorsque vous avez uniquement le service de rôle de serveur de Transport installé. Si vous avez à la fois le service de rôle de serveur de déploiement et le service de rôle de serveur de Transport installée (ce qui est la valeur par défaut), utilisez [à l’aide de la commande Nouveau MulticastTransmission](using-the-new-multicasttransmission-command.md). Notez que vous devez inscrire le fournisseur de contenu avant d’utiliser cette option.
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
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/ FriendlyName :<Friendly name>|Spécifie le nom convivial de l’espace de noms.|
|[/ Description :<Description>]|Définit la description de l’espace de noms.|
|/ Namespace :<Namespace name>|Spécifie le nom de l’espace de noms. Notez que cela n’est pas le nom convivial, et il doit être unique.<br /><br />-   **Service de rôle de serveur de déploiement**: La syntaxe de cette option est /Namespace:WDS :<Image group>/<Image name>/<Index>. Exemple : **WDS:ImageGroup1/install.wim/1**<br />-   **Service de rôle serveur de transport**: Cette valeur doit correspondre au nom donné lors de l’espace de noms a été créé sur le serveur.|
|/ ContentProvider :<Name>]|Spécifie le nom du fournisseur de contenu qui fournira le contenu pour l’espace de noms.|
|[/ ConfigString :<Configuration string>]|Spécifie la chaîne de configuration pour le fournisseur de contenu.|
|/ Namespacetype : {diffusion automatique &#124; par diffusion planifiée}|Spécifie les paramètres pour la transmission. Vous spécifiez les paramètres à l’aide des options suivantes :<br /><br />-[/ heure : <time>]-définit l’heure à laquelle la transmission doit démarrer en utilisant le format suivant : AAAA/MM/DD:hh:mm. Cette option s’applique uniquement aux transmissions de diffusion planifiée.<br />-[/ Clients : <Number of clients>]-définit le nombre minimal de clients à attendre avant le démarrage de la transmission. Cette option s’applique uniquement aux transmissions de diffusion planifiée.|
## <a name="BKMK_examples"></a>Exemples
Pour créer un espace de noms de diffusion automatique, tapez :
```
wdsutil /New-Namespace /FriendlyName:"Custom AutoCast Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
Pour créer un espace de noms de diffusion planifiée, tapez :
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:"Custom Scheduled Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider 
/Namespacetype:ScheduledCast /time:"2006/11/20:17:00" /Clients:20
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande get-AllNamespaces](using-the-get-allnamespaces-command.md)
[à l’aide de la commande remove-Namespace](using-the-remove-namespace-command.md) 
 [ Sous-commande : start-Namespace](subcommand-start-namespace.md)
