---
title: résolution
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88293422519488405d94e32596c81dbe4a697dee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371528"
---
# <a name="repair"></a>résolution

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

répare le volume RAID @ no__t-05 avec le focus en remplaçant la région de disque défaillant par le disque dynamique spécifié.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
| Paramètre  |                                                                                             Description                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| disque @ no__t-0 @ no__t-1  |                                                                 Spécifie le disque dynamique qui remplacera la région du disque défaillant.                                                                 |
| aligner @ no__t-0 @ no__t-1 |          Aligne toutes les étendues de volume ou de partition sur la limite d’alignement la plus proche. *n* est le nombre de kilo-octets \( Ko @ no__t-2 à partir du début du disque jusqu’à la limite d’alignement la plus proche.           |
|   noerr    | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |
  
## <a name="remarks"></a>Notes  
  
-   Le disque dynamique spécifié doit avoir un espace libre supérieur ou égal à la taille totale de la région du disque défaillant dans le volume RAID @ no__t-05.  
  
-   Vous devez sélectionner un volume dans un groupe RAID @ no__t-05 pour que cette opération aboutisse. Utilisez la commande **Sélectionner un volume** pour sélectionner un volume et lui déplacer le focus.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour remplacer le volume par le focus en le remplaçant par le disque dynamique 4, tapez :  
  
```  
repair disk=4  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

