---
title: créer une partition logique
description: Rubrique de référence pour Create partition Logical, qui crée une partition logique dans une partition étendue existante.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43d1c26d785fcecbb81a99c7484a15b3ea8b58a2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719235"
---
# <a name="create-partition-logical"></a>créer une partition logique

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une partition logique dans une partition étendue existante. Vous pouvez uniquement utiliser cette commande sur des disques MBR (Master Boot Record).

## <a name="syntax"></a>Syntaxe  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|  Paramètre  |                                                                                                                                                                                                                       Description                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  corps\=<n>  |                                                                                                              Spécifie la taille de la partition logique en \(mégaoctets\)(Mo), qui doit être plus petite que la partition étendue. Si aucune taille n’est indiquée, la partition se poursuit jusqu’à ce qu’il n’y ait plus d’espace libre dans la partition étendue.                                                                                                               |
| décalage\=<n> | Spécifie l’offset en kilo \(-\)octets (Ko), à partir duquel la partition est créée. Le décalage est arrondi pour remplir complètement la taille de cylindre utilisée. Si aucun décalage n’est spécifié, la partition est placée dans la première étendue de disque qui est suffisamment grande pour la contenir. La partition est au moins aussi longue en octets que le nombre spécifié par **taille\=**. Si vous spécifiez une taille pour la partition logique, celle-ci doit être plus petite que la partition étendue. |
| droite\=<n>  |                                                                                     Aligne toutes les étendues de volume ou de partition sur la limite d’alignement la plus proche. Généralement utilisé avec les groupes de LUN \(\) du numéro d’unité logique RAID matériel pour améliorer les performances.  <n>nombre de kilo-octets ( \(Ko\) ) entre le début du disque et la limite d’alignement la plus proche.                                                                                      |
|    noerr    |                                                                                                                           à des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                                                                                                           |
  
## <a name="remarks"></a>Notes   
  
-   Si les paramètres **taille** et **décalage** ne sont pas spécifiés, la partition logique est créée dans la plus grande étendue de disque disponible dans la partition étendue.  
  
-   Une fois la partition créée, le focus se déplace automatiquement vers la nouvelle partition logique.  
  
-   Vous devez sélectionner un disque MBR de base pour que cette opération aboutisse. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.  
  
## <a name="examples"></a>Exemples  
Pour créer une partition logique d’une taille de 1000 mégaoctets, dans la partition étendue du disque sélectionné, tapez :  
  
```  
create partition logical size=1000  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

