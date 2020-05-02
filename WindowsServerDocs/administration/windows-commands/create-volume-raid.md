---
title: créer un volume RAID
description: Rubrique de référence pour Create volume RAID, qui crée un volume RAID-5 à l’aide de trois disques dynamiques spécifiés ou plus.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15a165439d0b19023fa5270eb372a17af8d558a4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716935"
---
# <a name="create-volume-raid"></a>créer un volume RAID

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée un volume RAID-5 à l’aide de trois disques dynamiques spécifiés ou plus.  

> [!IMPORTANT]  
> Cette commande DiskPart n’est pas disponible dans les éditions de Windows Vista.

## <a name="syntax"></a>Syntaxe  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|           Paramètre           |                                                                                                                                                                                                                                              Description                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           corps\=<n>           | Quantité d’espace disque, en mégaoctets ( \(Mo\)), que le volume occupera sur chaque disque. Si aucune taille n’est donnée, le plus grand\-volume RAID 5 possible sera créé. Le disque avec le plus petit espace libre contigu disponible détermine la taille du volume RAID\-5 et la même quantité d’espace est allouée à partir de chaque disque. La quantité réelle d’espace disque utilisable dans le volume\-RAID 5 est inférieure à la quantité combinée d’espace disque, car une partie de l’espace disque est nécessaire pour la parité. |
| disque\=<n>,<n><n>,\[,<n>,...\] |                                                                                                                                               Disques dynamiques sur lesquels créer le volume RAID\-5. Vous devez disposer d’au moins trois disques dynamiques afin de créer un\-volume RAID 5. Une quantité d’espace égale à **la\= taille** est allouée sur chaque disque.                                                                                                                                                |
|          droite\=<n>           |                                                                                                                   Aligne toutes les étendues de volume sur la limite d’alignement la plus proche. Généralement utilisé avec les groupes de LUN \(\) du numéro d’unité logique RAID matériel pour améliorer les performances. *n* est le nombre de kilo- \(octets\) (Ko) à partir du début du disque jusqu’à la limite d’alignement la plus proche.                                                                                                                   |
|             noerr             |                                                                                                                                                 à des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                                                                                                                                  |
  
## <a name="remarks"></a>Notes   
  
-   Une fois le volume créé, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="examples"></a>Exemples  
Pour créer un volume\-RAID 5 d’une taille de 1000 mégaoctets, à l’aide des disques 1, 2 et 3, tapez :  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

