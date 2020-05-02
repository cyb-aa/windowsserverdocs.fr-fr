---
title: Obtient l’espace de noms
description: Pour obtenir des informations sur un espace de noms personnalisé, consultez la rubrique de référence sur l’espace de noms.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76980d2add9ee9b7584812c9d366408f8770b681
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719742"
---
# <a name="get-namespace"></a>Obtient l’espace de noms

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur un espace de noms personnalisé.

## <a name="syntax"></a>Syntaxe
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/Show:Clients]
```
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/details:Clients]
```
### <a name="parameters"></a>Paramètres

|               Paramètre               |                                                                                                                                                                                         Description                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      Joint<Namespace name>      | Spécifie le nom de l'espace de noms. Notez qu’il ne s’agit pas du nom convivial et qu’il doit être unique.<p>-Serveur de déploiement : la syntaxe du nom de l’espace de noms<ImageGroup>/<ImageName>/<Index>est/Namspace : WDS :. Par exemple : **WDS : ImageGroup1/install. wim/1**<br />-Serveur de transport : cette valeur doit correspondre au nom donné à l’espace de noms lorsqu’il a été créé sur le serveur. |
|        [/Server:<Server name>]        |                                                                                                             Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                              |
| [/Show : clients] ou [/details : clients] |                                                                                                                                                  Affiche des informations sur les ordinateurs clients qui sont connectés à l’espace de noms spécifié.                                                                                                                                                  |

## <a name="examples"></a>Exemples
Pour afficher des informations sur un espace de noms, tapez :
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
Pour afficher des informations sur un espace de noms et les clients qui sont connectés, tapez l’un des éléments suivants :
- Windows Server 2008 :`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2 :`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé](command-line-syntax-key.md)
  de syntaxe de ligne de commande[à l’aide de la commande](using-the-get-allnamespaces-command.md)
  AllNamespaces à l’aide de la
  [commande New-Namespace](using-the-new-namespace-command.md)[à l’aide de la commande Remove-Namespace](using-the-remove-namespace-command.md)
  , sous-[commande : Start-namespace](subcommand-start-namespace.md)
