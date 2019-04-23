---
title: Dfsdiag TestDCs
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 62956ae65d2311939ac0db6a4b86950f21dba407
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836600"
---
# <a name="dfsdiag-testdcs"></a>Dfsdiag TestDCs

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration de contrôleurs de domaine en effectuant les tests suivants sur chaque contrôleur de domaine dans le domaine spécifié :  
  
-   vérifie que le système de fichiers distribués \(DFS\) Namespace service est en cours d’exécution et que son type de démarrage est défini sur automatique.  
  
-   Vérifie la prise en charge du site\-évalué le coût de références pour NETLOGON et SYSvol.  
  
-   vérifie la cohérence de l’association du site par le nom d’hôte et l’adresse IP.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
dfsdiag /TestDCs [/Domain:<Domain name>]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|\/Domaine :<Domain name>|Domaine que vous souhaitez vérifier.|  
  
## <a name="remarks"></a>Notes  
\/Domaine est un paramètre facultatif. La valeur par défaut est le domaine local auquel l’hôte local est joint.  
  
## <a name="BKMK_Examples"></a>Exemples  
Pour vérifier la configuration de contrôleurs de domaine dans le domaine Contoso.com, tapez :  
  
```  
dfsdiag /TestDCs /Domain:Contoso.com  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [dfsdiag](dfsdiag.md)  
  

