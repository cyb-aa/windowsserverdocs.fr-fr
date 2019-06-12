---
title: À l’aide de la commande get-Namespace
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c30f9ef375bdf368f81f5a69961746851a2aac8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440430"
---
# <a name="using-the-get-namespace-command"></a>À l’aide de la commande get-Namespace

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="parameters"></a>Paramètres

|               Paramètre               |                                                                                                                                                                                         Description                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      / Namespace :<Namespace name>      | Spécifie le nom de l’espace de noms. Notez que cela n’est pas le nom convivial, et il doit être unique.<br /><br />-Déploiement Server : La syntaxe de nom de l’espace de noms est /Namspace:WDS :<ImageGroup>/<ImageName>/<Index>. Exemple : **WDS:ImageGroup1/install.wim/1**<br />-Transport Server : Cette valeur doit correspondre au nom donné à l’espace de noms lorsqu’il a été créé sur le serveur. |
|        [/Server:<Server name>]        |                                                                                                             Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                              |
| [/ Show : les Clients] ou [/ Détails : les Clients] |                                                                                                                                                  Affiche des informations sur les ordinateurs clients qui sont connectés à l’espace de noms spécifié.                                                                                                                                                  |

## <a name="BKMK_examples"></a>Exemples
Pour afficher des informations sur un espace de noms, tapez :
```
wdsutil /Get-Namespace /Namespace:"Custom Auto 1"
```
Pour afficher des informations sur un espace de noms et les clients qui sont connectés, tapez une des opérations suivantes :
- Windows Server 2008 : `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /Show:Clients`
- Windows Server 2008 R2 : `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /details:Clients`
  #### <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [à l’aide de la commande get-AllNamespaces](using-the-get-allnamespaces-command.md)
  [à l’aide de la commande Nouveau Namespace](using-the-new-namespace-command.md)
  [Using la commande remove-Namespace](using-the-remove-namespace-command.md)
  [sous-commande : Namespace-début](subcommand-start-namespace.md)
