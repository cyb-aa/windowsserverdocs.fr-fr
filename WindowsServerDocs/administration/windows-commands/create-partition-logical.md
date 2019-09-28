---
title: créer une partition logique
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f18048272eda710f7cb53a631ddeda81784a56b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378891"
---
# <a name="create-partition-logical"></a>créer une partition logique

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

crée une partition logique dans une partition étendue existante. Vous pouvez utiliser cette commande uniquement sur un enregistrement de démarrage principal \(MBR @ no__t-1 disques.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|  Paramètre  |                                                                                                                                                                                                                       Description                                                                                                                                                                                                                        |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  taille @ no__t-0 @ no__t-1  |                                                                                                              Spécifie la taille de la partition logique en mégaoctets \(MB @ no__t-1, qui doit être plus petite que la partition étendue. Si aucune taille n’est indiquée, la partition se poursuit jusqu’à ce qu’il n’y ait plus d’espace libre dans la partition étendue.                                                                                                               |
| décalage @ no__t-0 @ no__t-1 | Spécifie l’offset en kilo-octets @no__t-taille 0 Ko @ no__t-1, à partir duquel la partition est créée. Le décalage est arrondi pour remplir complètement la taille de cylindre utilisée. Si aucun décalage n’est spécifié, la partition est placée dans la première étendue de disque qui est suffisamment grande pour la contenir. La partition est au moins aussi longue en octets que le nombre spécifié par la **taille @ no__t-1 @ no__t-2**. Si vous spécifiez une taille pour la partition logique, celle-ci doit être plus petite que la partition étendue. |
| aligner @ no__t-0 @ no__t-1  |                                                                                     Aligne toutes les étendues de volume ou de partition sur la limite d’alignement la plus proche. Généralement utilisé avec le numéro d’unité logique RAID matériel @no__t 0LUN @ no__t-1 pour améliorer les performances.  <n> est le nombre de kilo-octets \( Ko @ no__t-2 à partir du début du disque jusqu’à la limite d’alignement la plus proche.                                                                                      |
|    noerr    |                                                                                                                           À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                                                                                                           |
  
## <a name="remarks"></a>Notes  
  
-   Si les paramètres **taille** et **décalage** ne sont pas spécifiés, la partition logique est créée dans la plus grande étendue de disque disponible dans la partition étendue.  
  
-   Une fois la partition créée, le focus se déplace automatiquement vers la nouvelle partition logique.  
  
-   Vous devez sélectionner un disque MBR de base pour que cette opération aboutisse. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour créer une partition logique d’une taille de 1000 mégaoctets, dans la partition étendue du disque sélectionné, tapez :  
  
```  
create partition logical size=1000  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

