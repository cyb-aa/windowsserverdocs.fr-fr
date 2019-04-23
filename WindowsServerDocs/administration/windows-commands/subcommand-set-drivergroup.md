---
title: Sous-commande DriverGroup de jeu
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e645e16a3d78dd91bad98fedbb04896025b0eaf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852700"
---
# <a name="subcommand-set-drivergroup"></a>Sous-commande : set-DriverGroup

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit les propriétés d’un groupe de pilotes existant sur un serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/ DriverGroup :<Group Name>|Spécifie le nom du groupe pilote.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si un nom de serveur n’est pas spécifié, le serveur local est utilisé.|
|[/ Nom :<New Group Name>]|Spécifie le nouveau nom pour le groupe pilote.|
|[/ Enabled : {Oui &#124; non}|Active ou désactive le groupe de pilotes.|
|[/Applicability:{Matched &#124; All}]|Spécifie lesquelles packages à installer si les critères de filtre n’est remplie. **Mise en correspondance** signifie installer uniquement les packages de pilotes qui correspondent à un matériel client s. **Tous les** signifie installer tous les packages aux clients, quel que soit leur matériel.|
## <a name="BKMK_examples"></a>Exemples
Pour définir les propriétés pour un groupe de pilotes, tapez une des opérations suivantes :
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[sous-commande : set-DriverGroupFilter](subcommand-set-drivergroupfilter.md)
