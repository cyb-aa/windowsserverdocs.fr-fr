---
title: À l’aide de la commande get-DriverGroup
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cfe10c3-a63f-48e7-bef9-f6b474b4ddbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f82969e03b3474cf39afd2ae5c3ef2f9d4f8b5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847150"
---
# <a name="using-the-get-drivergroup-command"></a>À l’aide de la commande get-DriverGroup

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur les groupes de pilotes sur un serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/ DriverGroup :<Group Name>|Spécifie le nom du groupe pilote.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet.  Si un nom de serveur n’est pas spécifié, le serveur local est utilisé.|
|[/Show: {PackageMetaData &#124; Filters &#124; All}]|Affiche les métadonnées pour tous les packages de pilotes dans le groupe spécifié. **PackageMetaData** affiche des informations sur tous les filtres pour le groupe pilote. **Filtres** affiche les métadonnées pour tous les packages de pilotes et des filtres pour le groupe.|
## <a name="BKMK_examples"></a>Exemples
Pour afficher des informations sur un fichier de pilote, tapez :
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Show:PackageMetaData
```
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Server:MyWdsServer /Show:Filters
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande get-AllDriverGroups](using-the-get-alldrivergroups-command.md)
