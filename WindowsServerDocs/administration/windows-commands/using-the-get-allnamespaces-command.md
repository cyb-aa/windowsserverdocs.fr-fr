---
title: AllNamespaces
description: La rubrique commandes Windows pour la commande AllNamespaces, qui affiche des informations sur tous les espaces de noms sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbdba81f68f609c6ed7ba740b68b1ac1123913ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831252"
---
# <a name="get-allnamespaces"></a>AllNamespaces

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur tous les espaces de noms sur un serveur.

## <a name="syntax"></a>Syntaxe
Windows Server 2008 :
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/Show:Clients] [/ExcludedeletePending]
```
Windows Server 2008 R2 :
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/details:Clients] [/ExcludedeletePending]
```
### <a name="parameters"></a>Paramètres

|         Paramètre         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/Server:<Server name>]  | Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé. |                        |
| [/ContentProvider :<name>] |                                                        Affiche les espaces de noms pour le fournisseur de contenu spécifié uniquement.                                                         |                        |
|      [/Show : clients]      |                            Pris en charge uniquement pour Windows Server 2008. Affiche des informations sur les ordinateurs clients qui sont connectés à l’espace de noms.                             |                        |
|    [/details : clients]     |                           Pris en charge uniquement pour Windows Server 2008 R2. Affiche des informations sur les ordinateurs clients qui sont connectés à l’espace de noms.                           |                        |
|  [/ExcludedeletePending]  |                                                              Exclut toutes les transmissions désactivées de la liste.                                                              |                        |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour afficher tous les espaces de noms, tapez :
```
wdsutil /Get-AllNamespaces
```
Pour afficher tous les espaces de noms, à l’exception de ceux qui sont désactivés, tapez :
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /details:Clients /ExcludedeletePending
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [à l’aide de la commande new-namespace](using-the-new-namespace-command.md)
  [à l’aide de la commande Remove-Namespace](using-the-remove-namespace-command.md)
  sous- [commande : Start-namespace](subcommand-start-namespace.md)
