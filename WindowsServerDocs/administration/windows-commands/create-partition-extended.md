---
title: création d’une partition étendue
description: Rubrique de référence pour Create partition Extended, qui crée une partition étendue sur le disque avec le focus.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b8af7247a9084b722f5b510df1d6af4622fc4ac2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719251"
---
# <a name="create-partition-extended"></a>création d’une partition étendue

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une partition étendue sur le disque qui a le focus. Vous pouvez utiliser cette commande uniquement sur les disques de l’enregistrement de démarrage principal (MBR).

## <a name="syntax"></a>Syntaxe  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|  Paramètre  |                                                                                                                             Description                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  corps\=<n>  |                                                  Spécifie la taille de la partition en \(mégaoctets\)(Mo). Si aucune taille n’est indiquée, la partition se poursuit jusqu’à ce qu’il n’y ait plus d’espace libre dans la partition étendue.                                                  |
| décalage\=<n> |                     Spécifie l’offset en kilo \(-\)octets (Ko), à partir duquel la partition est créée. Si aucun décalage n’est spécifié, la partition démarre au début de l’espace libre sur le disque qui est suffisamment grand pour contenir la nouvelle partition.                      |
| droite\=<n>  | Aligne toutes les étendues de partition sur la limite d’alignement la plus proche. Généralement utilisé avec les groupes de LUN \(\) du numéro d’unité logique RAID matériel pour améliorer les performances. <n>nombre de kilo-octets ( \(Ko\) ) entre le début du disque et la limite d’alignement la plus proche. |
|    noerr    |                                 à des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                 |
  
## <a name="remarks"></a>Notes   
  
-   Une fois la partition créée, le focus se déplace automatiquement vers la nouvelle partition.  
  
-   Une seule partition étendue peut être créée par disque.  
  
-   Cette commande échoue si vous tentez de créer une partition étendue au sein d’une autre partition étendue.  
  
-   Vous devez créer une partition étendue avant de pouvoir créer des lecteurs logiques.  
  
-   Vous devez sélectionner un disque MBR de base pour que cette opération aboutisse. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.  
  
## <a name="examples"></a>Exemples  
Pour créer une partition étendue de 1000 mégaoctets de taille, tapez :  
  
```  
create partition extended size=1000  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

