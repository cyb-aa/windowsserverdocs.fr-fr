---
title: Sous-commande Set-DriverGroup
description: La rubrique commandes Windows pour la sous-commande Set-DriverGroup, qui définit les propriétés d’un groupe de pilotes existant sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c49381dc65f3b2ffc9a04e4fb2699818515a931f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834032"
---
# <a name="subcommand-set-drivergroup"></a>Sous-commande : Set-DriverGroup

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit les propriétés d’un groupe de pilotes existant sur un serveur.

## <a name="syntax"></a>Syntaxe
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/DriverGroup :<Group Name>|Spécifie le nom du groupe de pilotes.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/Name :<New Group Name>]|Spécifie le nouveau nom du groupe de pilotes.|
|[/Enabled : {oui &#124; non}|Active ou désactive le groupe de pilotes.|
|[/Applicability : {correspondance trouvée &#124; }]|Spécifie les packages à installer si les critères de filtre sont satisfaits. **Correspondante** signifie que installe uniquement les packages de pilotes qui correspondent au matériel du client. **Tout** signifie d’installer tous les packages sur les clients, quel que soit leur matériel.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour définir les propriétés d’un groupe de pilotes, tapez l’une des options suivantes :
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
sous- [commande : Set-DriverGroupFilter](subcommand-set-drivergroupfilter.md)
