---
title: Dfsdiag TestDCs
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a193e68b6f015b1535a98e20b52deb2a4a14034c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378437"
---
# <a name="dfsdiag-testdcs"></a>Dfsdiag TestDCs

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration des contrôleurs de domaine en effectuant les tests suivants sur chaque contrôleur de domaine dans le domaine spécifié :  
  
-   vérifie que le système de fichiers DFS \(service d’espace de noms DFS\) est en cours d’exécution et que son type de démarrage est défini sur automatique.  
  
-   Vérifie la prise en charge des références de site\-coûtées pour NETLOGon et SYSvol.  
  
-   vérifie la cohérence de l’Association de sites par nom d’hôte et adresse IP.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|Domaine de \/:<Domain name>|Domaine que vous souhaitez vérifier.|  
  
## <a name="remarks"></a>Notes  
\/domaine est un paramètre facultatif. La valeur par défaut est le domaine local auquel l’hôte local est joint.  
  
## <a name="BKMK_Examples"></a>Illustre  
Pour vérifier la configuration des contrôleurs de domaine dans le domaine Contoso.com, tapez :  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

