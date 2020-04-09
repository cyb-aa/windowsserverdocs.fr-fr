---
title: DriverGroup
description: La rubrique commandes Windows pour la commande DriverGroup, qui affiche des informations sur les groupes de pilotes sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cfe10c3-a63f-48e7-bef9-f6b474b4ddbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60a0649e98a0af0cbbd12db9a07f97dade66344c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831092"
---
# <a name="get-drivergroup"></a>DriverGroup

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur les groupes de pilotes sur un serveur.

## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/DriverGroup :<Group Name>|Spécifie le nom du groupe de pilotes.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN).  Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/Show : {PackageMetaData &#124; Filters &#124; All}]|Affiche les métadonnées de tous les packages de pilotes dans le groupe spécifié. **PackageMetaData** affiche des informations sur tous les filtres pour le groupe de pilotes. **Filtres** affiche les métadonnées de tous les packages de pilotes et les filtres du groupe.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour afficher des informations sur un fichier de pilote, tapez :
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Show:PackageMetaData
```
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Server:MyWdsServer /Show:Filters
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande AllDriverGroups](using-the-get-alldrivergroups-command.md)
