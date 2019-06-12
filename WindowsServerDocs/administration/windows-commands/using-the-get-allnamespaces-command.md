---
title: À l’aide de la commande get-AllNamespaces
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b77bb80238ee63cc0d71d88592d75850720e33b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440523"
---
# <a name="using-the-get-allnamespaces-command"></a>À l’aide de la commande get-AllNamespaces

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur tous les espaces de noms sur un serveur.
## <a name="syntax"></a>Syntaxe
Windows Server 2008 :
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/Show:Clients] [/ExcludedeletePending]
```
Windows Server 2008 R2 :
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/details:Clients] [/ExcludedeletePending]
```
## <a name="parameters"></a>Paramètres

|         Paramètre         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/Server:<Server name>]  | Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé. |                        |
| [/ ContentProvider :<name>] |                                                        Affiche les espaces de noms pour le fournisseur de contenu spécifié uniquement.                                                         |                        |
|      [/Show:Clients]      |                            Uniquement pris en charge pour Windows Server 2008. Affiche des informations sur les ordinateurs clients qui sont connectés à l’espace de noms.                             |                        |
|    [/details:Clients]     |                           Uniquement pris en charge pour Windows Server 2008 R2. Affiche des informations sur les ordinateurs clients qui sont connectés à l’espace de noms.                           |                        |
|  [/ExcludedeletePending]  |                                                              Exclut les transmissions désactivées dans la liste.                                                              |                        |

## <a name="BKMK_examples"></a>Exemples
Pour afficher tous les espaces de noms, tapez :
```
wdsutil /Get-AllNamespaces
```
Pour afficher tous les espaces de noms, à l’exception de ceux qui sont désactivés, tapez :
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /details:Clients /ExcludedeletePending
  ```
  #### <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [à l’aide de la commande Nouveau Namespace](using-the-new-namespace-command.md)
  [à l’aide de la commande remove-Namespace](using-the-remove-namespace-command.md) 
   [ Sous-commande : start-Namespace](subcommand-start-namespace.md)
