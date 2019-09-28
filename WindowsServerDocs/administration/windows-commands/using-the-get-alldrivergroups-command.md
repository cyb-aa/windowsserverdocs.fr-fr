---
title: Utilisation de la commande AllDriverGroups
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bed6c784b2fafa30f2beb0394b64fe570ddd8ff7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363379"
---
# <a name="using-the-get-alldrivergroups-command"></a>Utilisation de la commande AllDriverGroups

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche des informations sur tous les groupes de pilotes sur un serveur.
## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/Show : {PackageMetaData &#124; Filters &#124; All}]|Affiche les métadonnées de tous les packages de pilotes dans le groupe spécifié. **PackageMetaData** affiche des informations sur tous les filtres pour le groupe de pilotes. **Filtres** affiche les métadonnées de tous les packages de pilotes et les filtres du groupe.|
## <a name="BKMK_examples"></a>Illustre
Pour afficher des informations sur un fichier de pilote, tapez :
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande DriverGroup](using-the-get-drivergroup-command.md)
