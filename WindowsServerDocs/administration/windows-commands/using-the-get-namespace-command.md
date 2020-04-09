---
title: Obtient l’espace de noms
description: Rubrique relative aux commandes Windows pour l’espace de noms obten-Namespace, qui affiche des informations sur un espace de noms personnalisé.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 58e84ec5c82ea3c2663b38bd2e53f65f2cf47519
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830882"
---
# <a name="get-namespace"></a>Obtient l’espace de noms

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur un espace de noms personnalisé.

## <a name="syntax"></a>Syntaxe
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/Show:Clients]
```
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/details:Clients]
```
### <a name="parameters"></a>Paramètres

|               Paramètre               |                                                                                                                                                                                         Description                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /Namespace :<Namespace name>      | Spécifie le nom de l’espace de noms. Notez qu’il ne s’agit pas du nom convivial et qu’il doit être unique.<p>-Serveur de déploiement : la syntaxe du nom de l’espace de noms est/Namspace : WDS :<ImageGroup>/<ImageName>/<Index>. Par exemple : **WDS : ImageGroup1/install. wim/1**<br />-Serveur de transport : cette valeur doit correspondre au nom donné à l’espace de noms lorsqu’il a été créé sur le serveur. |
|        [/Server:<Server name>]        |                                                                                                             Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                              |
| [/Show : clients] ou [/details : clients] |                                                                                                                                                  Affiche des informations sur les ordinateurs clients qui sont connectés à l’espace de noms spécifié.                                                                                                                                                  |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour afficher des informations sur un espace de noms, tapez :
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
Pour afficher des informations sur un espace de noms et les clients qui sont connectés, tapez l’un des éléments suivants :
- Windows Server 2008 : `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2 : `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [à l’aide de la commande AllNamespaces](using-the-get-allnamespaces-command.md)
  [à l’aide de la commande New-Namespace](using-the-new-namespace-command.md)
  [à l’aide de la commande Remove-Namespace](using-the-remove-namespace-command.md)
  sous- [commande : Start-namespace](subcommand-start-namespace.md)
