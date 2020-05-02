---
title: Sous-commande Set-DriverGroup
description: Rubrique de référence pour la sous-commande Set-DriverGroup, qui définit les propriétés d’un groupe de pilotes existant sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c70db688e17d185813298cea4fcee3b664f53d64
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721740"
---
# <a name="subcommand-set-drivergroup"></a>Sous-commande : Set-DriverGroup

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit les propriétés d’un groupe de pilotes existant sur un serveur.

## <a name="syntax"></a>Syntaxe
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/DriverGroup:<Group Name>|Spécifie le nom du groupe de pilotes.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/Name :<New Group Name>]|Spécifie le nouveau nom du groupe de pilotes.|
|[/Enabled : {Oui &#124; non}|Active ou désactive le groupe de pilotes.|
|[/Applicability : {matched &#124; All}]|Spécifie les packages à installer si les critères de filtre sont satisfaits. **Correspondante** signifie que installe uniquement les packages de pilotes qui correspondent au matériel du client. **Tout** signifie d’installer tous les packages sur les clients, quel que soit leur matériel.|
## <a name="examples"></a>Exemples
Pour définir les propriétés d’un groupe de pilotes, tapez l’une des options suivantes :
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
## <a name="additional-references"></a>Références supplémentaires
- [Sous-commande clé](command-line-syntax-key.md)
de syntaxe de ligne de commande[: Set-DriverGroupFilter](subcommand-set-drivergroupfilter.md)
