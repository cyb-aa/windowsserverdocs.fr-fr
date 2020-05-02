---
title: AllDriverGroups
description: Rubrique de référence pour la commande AllDriverGroups, qui affiche des informations sur tous les groupes de pilotes sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4ac3e0c7b05c96383714c3a702cffd6aa8df18a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720891"
---
# <a name="get-alldrivergroups"></a>AllDriverGroups

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur tous les groupes de pilotes sur un serveur.

## <a name="syntax"></a>Syntaxe
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/Show : {PackageMetaData &#124; Filters &#124; tout}]|Affiche les métadonnées de tous les packages de pilotes dans le groupe spécifié. **PackageMetaData** affiche des informations sur tous les filtres pour le groupe de pilotes. **Filtres** affiche les métadonnées de tous les packages de pilotes et les filtres du groupe.|
## <a name="examples"></a>Exemples
Pour afficher des informations sur un fichier de pilote, tapez :
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande DriverGroup](using-the-get-drivergroup-command.md)
