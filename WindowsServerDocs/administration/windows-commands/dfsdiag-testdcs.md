---
title: Dfsdiag TestDCs
description: La rubrique commandes Windows pour Dfsdiag TestDCs, qui vérifie la configuration des contrôleurs de domaine dans le domaine spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 092ce3710eb6d209f596683bd4ad054dadd11aa3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846314"
---
# <a name="dfsdiag-testdcs"></a>Dfsdiag TestDCs

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration des contrôleurs de domaine en effectuant les tests suivants sur chaque contrôleur de domaine dans le domaine spécifié :  
  
-   Vérifie que le service d’espace de noms système de fichiers DFS (DFS) est en cours d’exécution et que son type de démarrage est défini sur automatique.  
  
-   Vérifie la prise en charge des références de site à coût pour NETLOGon et SYSvol.  
  
-   Vérifie la cohérence de l’Association de sites par nom d’hôte et adresse IP.

## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
#### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|/Domain :`<domain_name>`|Domaine que vous souhaitez vérifier.|  
  
## <a name="remarks"></a>Notes  

/Domain est un paramètre facultatif. La valeur par défaut est le domaine local auquel l’hôte local est joint.  
  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Pour vérifier la configuration des contrôleurs de domaine dans le domaine Contoso.com, tapez :  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [Dfsdiag](dfsdiag.md)  
  

