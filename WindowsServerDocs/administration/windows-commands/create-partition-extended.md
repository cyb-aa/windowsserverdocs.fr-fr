---
title: création d’une partition étendue
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21620da46be0e1375f320172e7ccfe2edc338114
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378912"
---
# <a name="create-partition-extended"></a>création d’une partition étendue

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crée une partition étendue sur le disque qui a le focus. Vous pouvez utiliser cette commande uniquement sur un enregistrement de démarrage principal \(disques MBR\).  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|  Paramètre  |                                                                                                                             Description                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  taille\=<n>  |                                                  Spécifie la taille de la partition en mégaoctets \(Mo\). Si aucune taille n’est indiquée, la partition se poursuit jusqu’à ce qu’il n’y ait plus d’espace libre dans la partition étendue.                                                  |
| décalage\=<n> |                     Spécifie le décalage en kilo-octets \(Ko\), à partir duquel la partition est créée. Si aucun décalage n’est spécifié, la partition démarre au début de l’espace libre sur le disque qui est suffisamment grand pour contenir la nouvelle partition.                      |
| aligner\=<n>  | Aligne toutes les étendues de partition sur la limite d’alignement la plus proche. Généralement utilisé avec le numéro d’unité logique RAID matériel \(les groupes de LUN\) pour améliorer les performances. <n> est le nombre de kilo-octets \(Ko\) à partir du début du disque jusqu’à la limite d’alignement la plus proche. |
|    noerr    |                                 à des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                 |
  
## <a name="remarks"></a>Notes  
  
-   Une fois la partition créée, le focus se déplace automatiquement vers la nouvelle partition.  
  
-   Une seule partition étendue peut être créée par disque.  
  
-   Cette commande échoue si vous tentez de créer une partition étendue au sein d’une autre partition étendue.  
  
-   Vous devez créer une partition étendue avant de pouvoir créer des lecteurs logiques.  
  
-   Vous devez sélectionner un disque MBR de base pour que cette opération aboutisse. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour créer une partition étendue de 1000 mégaoctets de taille, tapez :  
  
```  
create partition extended size=1000  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

